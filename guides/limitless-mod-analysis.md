# Limitless (mod analysis)

!!! info "v1.4.8"

Reverse-engineering notes for the community **Limitless** module: how it raises battle agent capacity beyond the stock native pool. Independent of any other total-conversion content.

Related: [Agents](/modding/agents/), [Missions](/modding/missions/), [MissionLogic](/modding/missionlogic/), [MBSubModuleBase](/modding/mbsubmodulebase/), [Harmony](/modding/harmony/).

---

## What it is

Limitless increases the **native agent slot pool** used in single-player missions (soldiers, player, mounts). Stock capacity is exposed to C# as:

``` cs
// DefaultBattleMissionAgentSpawnLogic
public static int MaxNumberOfAgentsForMission
{
    get
    {
        if (_maxNumberOfAgentsForMissionCache == 0)
            _maxNumberOfAgentsForMissionCache = MBAPI.IMBAgent.GetMaximumNumberOfAgents();
        return _maxNumberOfAgentsForMissionCache;
    }
}

public static int MaxNumberOfTroopsForMission => MaxNumberOfAgentsForMission / 2;
```

`GetMaximumNumberOfAgents()` reads the engine limit baked into `TaleWorlds.Native.dll`. Harmony on managed spawn code alone cannot grow that pool — Limitless patches the native binary in memory, then retargets campaign spawn math to spend the new slots.

It is **not** a skill/attribute-cap mod.

---

## Layout on disk

Under `Modules\Limitless\`:

| Path | Role |
|---|---|
| `SubModule.xml` | Registers `Limitless.dll` / `Limitless.SubModule` |
| `ModConfig.json` | Player settings (JSON) |
| `bin\Win64_Shipping_Client\Limitless.dll` | Managed submodule + Harmony |
| `bin\Win64_Shipping_Client\0Harmony.dll` | Bundled Harmony |
| `System\Limitless.ini` | Native-facing copy of a subset of settings |
| `System\Limitless.Injector.exe` | Loads the patcher into the live game process |
| `System\Limitless.Patcher.dll` | Native recipes against `TaleWorlds.Native.dll` |

`SubModule.xml` dependencies on the analyzed build (optional CustomBattle):

``` xml
<DependedModules>
  <DependedModule Id="Native" DependentVersion="v1.4.8" />
  <DependedModule Id="SandBoxCore" DependentVersion="v1.4.8" />
  <DependedModule Id="CustomBattle" Optional="true" DependentVersion="v1.4.8" />
  <DependedModule Id="Sandbox" DependentVersion="v1.4.8" />
</DependedModules>
```

---

## Settings

`ModConfig.json` (managed) drives the Options UI and is rewritten to `System\Limitless.ini` before native init.

| Key | Meaning |
|---|---|
| `AgentLimit` | Total native slots (soldiers + player + horses). Multiple of **30**, range **2040–32760** |
| `SiegeAgentLimit` | Cap for siege / sally-out spawn budgets (≤ `AgentLimit`) |
| `RealUnlimitedCorpses` | Raises corpse visual/pool caps (native policy uses **16380**) |
| `CustomAnimationSampling` | Overrides distant animation sampling |
| `DistantAnimationIntervalMs` | Far-distance interval (ms), **1–1000** when custom sampling is on |
| `DespawnHorses` | Mission behavior fades eligible riderless mounts |
| `DespawnHorseDelaySeconds` | Minimum riderless time before fade eligibility |

Native INI only carries the keys the patcher needs (`AgentLimit`, corpse/anim flags). Horse despawn stays managed-only.

Changing settings requires a **full game restart**. Mid-session retries are refused.

---

## Boot sequence

1. Game loads the module. Startup runs from `OnBeforeInitialModuleScreenSetAsRoot` (fresh main menu). It aborts if a `Game` or `Mission` already exists, or in multiplayer.
2. Managed code loads `ModConfig.json` and writes `System\Limitless.ini`.
3. If `Limitless.Patcher.dll` is not already loaded in-process, it starts:
   `Limitless.Injector.exe --pid <current PID> "<…>\System\Limitless.Patcher.dll"`.
4. Injector waits until `TaleWorlds.Native.dll` is mapped, then loads the patcher (remote `LoadLibraryW`). Target must be Bannerlord / launcher; a per-PID mutex blocks duplicate loads.
5. Managed code resolves patcher exports and calls `Limitless_InitializeSinglePlayerFromMenu`.
6. Capacity / feature bits / animation interval are checked against config and against `DefaultBattleMissionAgentSpawnLogic.MaxNumberOfAgentsForMission`.
7. Harmony installs campaign spawn budgeting and Performance Options entries. Optional `HorseCleanupLogic` is added on mission start when despawn is enabled.

If inject/init fails, campaign expansion is disabled for that session; any native patches that already applied stay until restart.

---

## Native patcher

`Limitless.Patcher.dll` is unmanaged (x64). Public exports:

| Export | Purpose |
|---|---|
| `Limitless_Initialize` | Generic init entry |
| `Limitless_InitializeSinglePlayerFromMenu` | Menu-gated SP init (what the submodule calls) |
| `Limitless_GetCapacity` | Active agent-slot capacity after patch |
| `Limitless_GetFeatures` | Bit flags (corpses / custom anim) |
| `Limitless_GetAnimationIntervalMs` | Active distant-anim interval |
| `Limitless_GetStatus` | ANSI status string for diagnostics |

Observed init behavior (from status strings / imports):

1. **SHA256** of on-disk `TaleWorlds.Native.dll` (`BCrypt`). Unknown hash → `Unsupported engine SHA256` — recipes are build-specific.
2. Confirm loaded `.text` matches the file (refuse if another native patcher already mutated it).
3. **Suspend threads**, apply a recipe/manifest of immediate and RIP-relative operand patches (stock sizes such as **2040** → configured `AgentLimit`), restore protection, resume threads.
4. Optional corpse and animation recipes when those INI flags are set.

Recipe labels seen in the binary include Mission allocation, primary/second agent pools, link tables and counters, slot init/fill counts, Agent-ID bit packing (stock: `min=-1, max=2046, bits=11`), bone matrix/mapping block growth, corpse renderer pool sizes, and private RIP-relative animation denominators.

### Why so many sites?

Community confirmation ([BUTR] Aragas): the C++ engine **hardcodes** the battle max-size / agent-pool length as an **inlined constant**, not a single runtime variable. Compilers duplicate that immediate across allocations, loops, ID packing, and related geometry — on the order of **~1600 occurrences** in a typical build (ballpark, not a fixed contract). Limitless (and equivalent experiments) locate each relevant immediate and rewrite it.

That is exactly why the mod is **one game version at a time**:

- Every Bannerlord / native rebuild reshuffles code and immediates → new offsets / recipes.
- There is **no** stable Harmony-style hook for “change one capacity field and inherit forever.”
- Backwards-compatible tooling that must survive patches generally cannot ship this approach; maintainers re-run the find-and-patch pass per update (community estimate: often under an hour with modern AI-assisted scanning, still mandatory each release).

A cleaner engine-side fix would be a **mutable static (or config-driven) capacity** instead of a compile-time constant, so one write updates all consumers. Whether TaleWorlds adopts that is unknown; until then, inlined-constant relocation remains the only practical path for raising the native ceiling.

---

## Managed Harmony layer

After native capacity exists, C# must **budget** those slots for campaign / custom battles.

Targets (campaign / mission names verified in 1.4.8 decompiled game source):

- `SandBoxMissions.CreateCampaignMissionAgentSpawnLogic`
- `DefaultBattleMissionAgentSpawnLogic` (`_battleSize`, `InitWithSinglePhase`, quota helpers)
- `MissionBattleSideSpawnContext` (`TryReinforcementSpawn`, `GetReservedTroopQuota`, `SpawnTroops`)
- `BannerlordMissions.OpenCustomBattleMission` / `OpenSiegeMissionWithDeployment`
- Custom Battle UI: `ArmyCompositionGroupVM.MaxArmySize` when `TaleWorlds.MountAndBlade.CustomBattle` is loaded (shipping assembly; not always present in every source dump)

Behavior summary:

- Registers a per-logic **slot budget** = `AgentLimit` or `SiegeAgentLimit`, clamped to native capacity; writes `_battleSize`.
- Transpilers replace reads of `MaxNumberOfAgentsForMission` with that budget where quota = capacity − `NumberOfAgents`.
- Mounted troops cost **2** slots when horses spawn; foot cost **1**.
- Initial deployment binary-searches a battle size so defender + attacker composition fits remaining slots.
- Custom Battle army size ≈ `AgentLimit / 2`.

Options UI: Harmony adds a **Limitless (restart required)** group under Performance (`OptionsProvider.GetPerformanceOptionCategory` and related `OptionsVM` hooks).

Horse cleanup: a `MissionLogic` scans `Mission.MountsWithoutRiders`, skips mounts near mount interactions / reserved riders, then `FadeOut` after the configured delay — only on supported campaign battles that have a registered budget.

---

## Architecture

```
ModConfig.json
      │
      ▼
Limitless.dll (main menu)
      │
      ├── write System\Limitless.ini
      ├── spawn Limitless.Injector.exe --pid …
      │         └── LoadLibrary → Limitless.Patcher.dll
      │                    └── patch TaleWorlds.Native.dll in memory
      │                         (pools / IDs / corpses / anim)
      └── Harmony: spawn budgets + Options + horse fade
```

---

## Practical limits

- **SP only.** Multiplayer paths are refused.
- Init only at the **idle main menu** (no live `Mission`).
- Hostile to **other native patchers** that rewrite `TaleWorlds.Native.dll` `.text`.
- High `AgentLimit` costs RAM/CPU; siege scenes can still trap agents when the scene cannot hold the budget.
- `AgentLimit` must stay a multiple of 30; that matches how pool geometry is scaled in the recipes.

---

## Takeaways for modders

1. Managed battle-size mods that only change spawn counts still hit `MBAPI.IMBAgent.GetMaximumNumberOfAgents()`.
2. Raising that ceiling means **native** work (or shipping a forked engine), not Harmony alone — and specifically **relocating an inlined constant** across a large recipe set (~thousand-scale sites), not flipping one global.
3. Any native recipe set is tied to a specific `TaleWorlds.Native.dll` hash — plan for **per-patch** maintenance; this will not be backwards-compatible across game updates.
4. After growing the pool, still retarget campaign spawn / reinforcement math so mounts and both sides fit the new budget.
5. Prefer asking TW for a **static / configurable** max-agent capacity if the goal is long-lived modding APIs; patching inlined immediates is workable but forever version-locked.
