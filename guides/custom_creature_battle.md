# Custom Creature: big creatures in battle

What changes when a creature or race is much bigger than a human: its size, collision and weapons,
then the code behind its attacks, formation spacing and banners, and why an area attack needs a cap.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. Read [the XML](/guides/custom_creature_xml/)
first, or for a race, [the race page](/guides/custom_creature_race/).

!!! note "Version"
    Measured against **Bannerlord v1.5.3**. The engine-code findings come from TAOM's reverse
    engineering of the v1.5.3 game and Modding Kit DLLs, and native crash offsets move with every
    engine update. Older measurements (v1.4.5 to v1.4.7) are marked where they appear.

!!! note "The last four sections need a C# module"
    Size, collision and weapons are data you author in XML, Blender and the Modding Kit. Scripted
    attacks, area caps, formation spacing and the banner gate each change what engine code does, so
    XML alone cannot do them: they take a [mission logic](/modding/missionlogic/), a
    [game model](/modding/models/) override or, for formation spacing and one banner path,
    [Harmony](/modding/harmony/) patches. Each of those sections says what you get if you skip it.

## Size

The engine has no size on a Monster. A mount takes its size from its Horse item: at build the engine
calls `SetInitialAgentScale(0.01 x body_length)` (v1.5.3 managed code), which scales the skeleton,
capsules and ragdoll together. 100 is the authored size; TAOM's mumakil, its war elephant at 3x, is
300. So a mount's Monster capsule is written at 1x: the mumakil's, radius 1.1 in its XML, is about
3.3 in game.

!!! warning "`body_length` does not scale the rider"
    The managed code reads as if it would (the Horse slot and `ArmorItemEndSlot` share an index, and
    the scale block has no mount check), but the trace ends in native code, and in game the rider is
    not scaled: a 3x mumakil's rider stands at 1x beside its 1x crew. Older advice says the
    opposite.

Anything your own code places against the creature (an attack's reach, a seat offset) does not scale
by itself: read `Agent.AgentScale` and multiply. The exception is a prefab with a physics body or a
navmesh: TAOM builds its mumakil tower at final size, having found no vanilla one scaled at runtime.

A race is scaled by its skin in `skins.xml`: TAOM's cave troll, on `human_skeleton` with
`min_scale="1.9"`, spawns at about 1.9x. Whether that scale grows the Monster's capsule as
`body_length` does is not established; the cave troll kept Native's 0.37 capsule and fought.
**A race on its own skeleton carries its size in the skeleton, so `AgentScale` alone under-reads
it.** TAOM takes body size as `AgentScale` times the Monster's `standing_eye_height` over the
human's 1.70. TAOM's hill troll, a race on its own skeleton, has an eye height of 3.58;
`AgentScale` alone would have given it about half the cave troll's reach.

`Items.xsd` requires `body_length` on `<Horse>`, and a module cannot extend engine schemas. TAOM's
per-Monster size is its own attribute, copied into each Horse item by its code: one approach, not an
engine feature, untested in game.

## Three collision layers

A creature collides in three places. Two live in its skeleton, one record per bone, inside the tpac
(the binary package the Modding Kit, TaleWorlds' asset editor, builds from your FBX):

| Layer | Where it lives | What it does |
|---|---|---|
| **Body capsule** | `<Capsules><body_capsule>` in the Monster XML | agents bump into it and path around it |
| **Hit capsule**, per bone | `CollisionPosition1`/`2`, `CollisionRadius`, a body zone | weapons and missiles strike it |
| **Ragdoll capsule**, per bone | `RagdollPosition1`/`2`, `RagdollRadius`, masses, joints | the corpse only |

**When someone says the collision is too small, ask first: do units walk into it, or do blows pass
through it?** Walking in is the body capsule; blows passing through are the hit capsules.

!!! note "Not verified in game"
    That weapons meet the hit capsule, not the ragdoll one, is inferred from the field names and the
    body zones, which match the [`BoneBodyPartType`](/modding/agents/#bonebodyparttype) a blow reads
    for its armour zone. No in-game hit test has confirmed it on any TAOM creature.

### The Kit's default hit capsule is almost nothing

A skeleton nobody authored gets a rod along each bone's local x from 0.1 to 1.0 of its length,
radius a tenth of that length (the tell: a radius about a ninth of the capsule's length). Nothing
looks broken. The share of the skin inside any hit capsule:

| Skeleton | Default hit capsules | Skin covered | After fitting to the mesh |
|---|---|---|---|
| TAOM's war elephant | 41 of 60 | 48% | **98.2%** |
| TAOM's giant spider | 40 of 62 | 49.9% | **97.2%** |
| TAOM's chariot (two horses and a cart on one skeleton) | 57 of 60 | 8.8% | **98.0%** |
| the warg (Byak0's Alliance.Wargs) | 2 of 49 | not measured | not refitted |

The warg's other 47 capsules were already authored, so TAOM did not refit it and has no coverage
figure for it.

The fit rules that mattered: let a capsule stand at most 20 cm past the skin (90th percentile) and
leave the rest to its neighbours; try its ends at the skin's extent; take its axis from the bone,
since a fixed local x lies across every limb of a rig exported along +Y.

Optional helpers: [`export_skin_for_capsules.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/export_skin_for_capsules.py)
exports the skin from headless Blender, and
[`skeleton_hit_capsules.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/skeleton_hit_capsules.py)
fits capsules to it and patches the tpac (the Kit's skeleton editor does it by hand). After an
offline patch, save the module once in the Kit so it re-cooks the package's
[RuntimeDataCache](/guides/custom_creature_clip_inspector/#what-save-writes-and-the-runtimedatacache)
entry; whether the game reads the capsules from the package or from that entry is not established.

### Size the body capsule from the mesh

This is for a creature or race not built like a human, such as a race on its own skeleton: measure
the mesh's shoulders and hips rather than scaling a human capsule. On `human_skeleton`, TAOM's cave
troll kept the human's and fought ([size](#size)). The hill troll got the human capsule scaled by
height (radius 0.82), but it is about twice as broad for its height (2.43 m shoulders) and
neighbours stood inside each other. TAOM's `monsters.xml` now gives it half the shoulders:

```xml
<Capsules>
	<body_capsule radius="1.2"
		pos1="0.0, 0.0, 3.05"
		pos2="0.0, 0, 2.15" />
</Capsules>
```

The mumakil first shipped the elephant's capsule; enemies ran into the visible mesh before meeting
it. A platform needs the opposite care: the war elephant's capsule stays under its howdah floor
(3.2 m at 1x), since a floor inside it is the leading, unconfirmed cause of its sliding.

**Check it** in a Custom Battle: swing at and shoot the neck, legs and haunches, and walk infantry
into it. For a scripted sweep, `Mission.RayCastForClosestAgentsLimbs` (v1.5.3) returns the agent
and bone a ray hits; TAOM has not used it yet.

## Weapons for a large race

**Crafted weapons store no damage.** The engine simulates swing speed and damage from each piece's
geometry and weight (`TaleWorlds.Core.Crafting`). TAOM's hill troll hammer, a longer, heavier clone
of the cave troll's two-handed mace, came out at 23 Blunt and swing speed 12 against the mace's 86
and 28; with the head's `weight` cut from 1.23 to 0.875 it came out at 86 and 28. Those figures come
from TAOM's Python copy of the engine's crafting simulation, not from the in-game tooltip. See
[Weapons](/modding/weapons/).

* When a crafting piece has a `length`, the engine sets both its piece distances to half of it, so
  `distance_to_previous_piece` is dead data.
* A `body_name` that no package ships hangs the preload: save the weapon's meshes and `bo_` physics
  body in the Kit before any item names them.

A race on its own skeleton holds items on grip bones: TAOM's hill troll Monster names `r_finger0` in
`main_hand_item_bone` and `l_finger0` in `off_hand_item_bone`. Clips do not need them; weapons do
(the hammer sits in the troll's hand in game). A grip bone carried from a donor's proportions can miss
the fist (see [the race's skeleton](/guides/custom_creature_race/#the-skeleton-human-names-human-order-human-axes)).
Armour follows the skeleton too: armour rigged to `human_skeleton` floats on another rig.

Optional helper: [`melee_catalogue.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/melee_catalogue.py)
runs that simulation over every crafted weapon in a `Modules` folder without launching the game,
read-only.

## Scripted creature attacks

**Needs C#, no Harmony.** The engine gives a mount no attack beyond charge damage and its usage
set's `kick_action`, which it fires by itself: vanilla's horse set fires `act_horse_kick` at
whatever stands behind the mount, and the kick clip's combat parameter, `horse_kick_params`, drives
the engine's own hit detection. A new creature's usage set can name its own kick action, but no TAOM
test records one firing or landing: treat that route as unverified. **An authored attack clip does
not attack by itself:** a bite or trample is code that plays the action and applies a blow. Skip
this section and a mount fights by charge damage and, at most, its kick. A race's swings use the
engine's melee instead: see
[melee attack clips](/guides/custom_creature_melee/#two-kinds-of-attack).

The pattern behind every TAOM creature:

* **Attach per agent, by Monster.** A [mission logic](/modding/missionlogic/) attaches a behaviour
  tree to every agent whose `Monster.StringId` matches, on the first tick and again from
  `OnAgentBuild`, because Custom Battle deployment spawns agents later. A mount agent has no
  character to key on. The tree layers on the engine's mount AI and needs no Monster flag
  ([never `CanAttack`](/guides/custom_creature_xml/#the-attributes-that-are-not-free-choices)).
* **Derive it from `MissionLogic`.** The engine code (read on v1.4.5, unchanged on v1.5.3) files a
  behaviour reporting `MissionBehaviorType.Logic` under its logics with an `as MissionLogic` cast;
  one that is not a `MissionLogic` becomes a null there, and `CheckMissionEnded` throws.
* **Resolve actions once at mission start and log any that come back `act_none`.** On channel 0, the
  full-body channel locomotion uses, `act_none`
  [freezes the legs while the agent slides](/guides/custom_creature_troubleshooting/#the-creature-slides-with-its-legs-frozen).
  Never touch `ActionIndexCache` before the engine has loaded `action_types.xml`: on v1.4.7 an early
  touch made its static constructor bake every value to -1 for good.
* **Request with a priority that wins, and hit on the measured frame.** The Kit's default priority 0
  can lose to locomotion in battle (TAOM's reading;
  [why](/guides/custom_creature_clip_inspector/#priority-why-a-clip-plays-in-the-viewer-and-not-in-battle));
  [`SetActionChannel`](/modding/animations/#setactionchannel) can replace it per request. Deliver
  the blow at the clip progress where the weapon lands (0.58 on TAOM's troll smash). TAOM's war ram,
  a horse-skeleton reskin that dwarves ride, with one authored head-butt clip, logged 1,300
  head-butts whose clip never showed: a damage log proves nothing about the animation.
* **Damage written as already calculated skips armour.** TAOM's creature blows set `InflictedDamage`
  with `DamageCalculated = true`: 976 ram head-butts on armoured trolls averaged 23.1, the middle of
  the raw 18 to 28 roll. A Blunt killing blow needs `CanKillEvenIfBlunt` or it only wounds. Give the
  blow to the rider while there is one.
* **Take strike bones from the creature's own skeleton.** The warg's bone indices reused on the
  spider connected 2 of 75 attacks.
* **Agent slots are reused and some callbacks run off-thread.** Never key on `Agent.Index`, recheck a
  held agent's identity, and write engine state on the main thread.
* **Mount lock.** Override `AgentStatCalculateModel.CanAgentRideMount` to refuse the creature and set
  its `MountDifficulty` to 999; the assigned rider's spawn ignores both. Pattern from Artem's
  ADOD_Beasts; no in-game "cannot steal it" test is recorded.
* **The rider's `default_group` decides whether it closes.** As `HorseArcher` TAOM's elephant
  skirmished and never trampled; as `Cavalry` it charges.
* **Riderless is possible, unsupported.** `Mission.SpawnMonster` builds one, and `Agent.SetTeam` and
  the `Formation` setter field it (ADOD_Beasts' riderless wolves work this way), with no engine AI or
  roster support. If your creature carries a rider, make it a mount: see the index's
  [warning against detached non-mount agents](/guides/custom_creatures/#what-a-creature-is-to-the-engine).

## Cap area attacks

**Needs C#.** The engine has no area melee (one swing, one target), so the attack and its cap are
both code. Skip the cap and a scripted area hit multiplies across a stack: before its cap, one troll
smash cleared a whole infantry block. The war ram first swept a 3.5 m knockdown on a 6 s cooldown;
fifteen to a lord's party, it played as overpowered. The radius went to 2 m and the cooldown to
10 s, a battle log still showed several victims per butt, and the butt now hits one enemy.
**Budget an area attack by how many of the creature a battle fields.**

| Attack | Shape | Status |
|---|---|---|
| War ram head-butt | the enemy it faces most squarely within 2 m (a rider, never his horse) | single-target version not yet seen in game |
| Elephant and mumakil trample | radial, a fixed radius set by hand for each size: 4 m on the elephant, 12 m on the 3x mumakil | fought in battles |
| Troll smash | capped: rolls 1 to 5, hits that many nearest the impact | current tuning, not seen in game |

The capped pattern, as TAOM's troll smash does it:

1. Gather the enemies within 3.5 m of an impact point 1.8 m ahead (`Mission.GetNearbyEnemyAgents`),
   skipping mounts (or take the rider, as the ram does).
2. Roll this smash's cap from `MBRandom.RandomFloat`, evenly from 1 to 5.
3. Sort by distance from the impact and hit the nearest N; the rest are untouched.
4. Full damage inside 1.5 m, falling to about a ninth at the edge; a shield-blocking victim
   (`GetCurrentActionType(1)` is `DefendShield`) takes half and stays up.
5. Multiply every distance by the attacker's [body size](#size), capped at 3 (three times the
   human's size).

**Check it** by logging each smash's rolled cap and victims in a battle.

!!! note "Not verified in game"
    The troll smash cap and its numbers are TAOM's current tuning: unit-tested, not yet seen in a
    battle.

## Formation spacing

**Needs C# and Harmony.** The engine spaces every foot unit for a 0.76 m human:
`Formation.UnitDiameter` is `BipedalRadius` times two, whatever the Monster, so no Monster or race
XML widens it. Skip this section and broad units stand inside each other, as TAOM's trolls did. One
approach that worked, as [Harmony](/modding/harmony/) patches:

1. **A postfix on the `Formation.UnitDiameter` getter** returns a wider width once broad units are a
   tenth of the formation, the share at which the engine code spaces a formation for horses. Past it
   every unit is spaced wide, humans beside the trolls included.
2. **The width comes from the mesh:** LOD0 shoulder width times `AgentScale`, capped at 4 m. TAOM's
   cave troll gets about 1.43 m, the hill troll about 2.7 m.
3. **Compute on the main thread, read anywhere.** The getter runs on other threads too, so the
   postfix only reads a thread-safe table. Key it by reference: `Formation.GetHashCode` reads `Team`,
   null on the layout copies of step 5.
4. **Rebuild the cached slots** when the width changes (`Formation.OnUnitAddedOrRemoved()`, then
   `Arrangement.OnFormationFrameChanged(updateCachedOrderedLocalPositions: true)`), and during
   deployment replay the tail of `Formation.OnMassUnitTransferEnd` so placed units jump to the new
   slots.
5. **Let layout copies borrow the width.** The order preview, deployment placement and spawn frames
   lay a formation out on a team-less copy through `GetUnitPositionWithIndexAccordingToNewOrder` and
   `GetUnitSpawnFrameWithIndex`; a prefix and finalizer there name the real formation for the call.

**Check it** on the deployment screen: broad units should stand apart, as in TAOM's final battle
test. Known gaps: the custom-width order preview reads one width outside that scope (not verified in
game), and the deployment plan sizes spawn areas with the static `Formation.GetDefaultUnitDiameter`.
See [Formations](/modding/formations/).

## Banners, and other jobs meant for humans

**Needs C#, and one Harmony patch.** `Agent.IsHuman` tests the humanoid flag, not the human race,
so every `IsHumanoid="true"` race can bear a banner and do every other job gated on `IsHuman`. Gate
the job, not `IsHuman`: here, with banner-bearer [model](/modding/models/) overrides, since a kit
can keep only one door shut. Skip it and the race carries standards: TAOM's hill trolls raised them
in a Custom Battle while only the campaign's model was gated.

The campaign and Custom Battle use different banner-bearer models: Custom Battle builds
`CustomBattleBannerBearersModel` on a `BasicGameStarter`, so an override of the campaign's
`SandboxBattleBannerBearersModel` never runs there. A race gate has four doors to close:

1. **`CanAgentBecomeBannerBearer`**, which picks bearers at deployment.
2. **`CanAgentPickUpAnyBanner`**, for a dropped banner. The fetch never asks door 1, and vanilla's
   version checks `IsHuman`, not the race.
3. **The item-pickup tick.** `DefaultItemPickupModel` accepts the agent's own formation banner into an
   empty slot without asking door 2. The tick runs for an agent that lost its spawn shield or holds a
   consumable stack at half or less, **so a big race's kit with a shield or any consumable, arrows
   included, reopens it.**
4. **The reinforcement spawn.** `MissionBattleSideSpawnContext.SpawnTroops` checks only `!IsHero`
   before `SpawnBannerBearer`; closing it takes a Harmony patch.

**Check it** in a battle with reinforcements. In TAOM's final battle test no troll carried a banner;
kits with no shield or consumable keep door 3 shut.

## Next

[A humanoid race on its own skeleton](/guides/custom_creature_race/) for the trolls' skeleton and
grip bones, and [troubleshooting](/guides/custom_creature_troubleshooting/) when something fails.
