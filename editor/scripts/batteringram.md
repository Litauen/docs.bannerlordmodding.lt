# BatteringRam

!!! info "v1.4.8"

The ram that rolls up to a gate and hits it. Scenes place a [BatteringRamSpawner](/editor/scripts/batteringramspawner/), which spawns this prefab. The weapon prefab is hidden in the prefab browser.

Find placed ones in the Scene panel with `s:BatteringRam`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the spawner in a siege scene. The ram looks for a path entity and a [CastleGate](/editor/scripts/castlegate/) whose tags the spawner copied across.

## What you set

`Native\Prefabs\props_siege_batteringrams.xml` stores this order on `batteringram_a`. The prefab also writes **ForcedUse** `false`. That property is hidden from the panel.

| Panel field | What to do with it |
|---|---|
| **GhostEntityMove** | Editor ghost motion. Code default is on. The prefab stores off. |
| **GhostEntitySpeedMultiplier** | Scales that ghost. The prefab stores `1`. |
| **WheelDiameter** | Wheel size used while it rolls. The prefab stores `1.3`. |
| **GateNavMeshId** | Navmesh id of the gate opening. Code default is `7`. The prefab stores `50`. |
| **DisabledNavMeshID** | Navmesh id disabled with the ram. Code default is `8`. The prefab stores `40`. |
| **NavMeshIdToDisableOnDestination** | Face id turned off when the ram arrives. The prefab stores `-1`. |
| **MinSpeed** | Slowest roll. Code default is `0.5`. The prefab stores `0.25`. The spawner multiplies this. |
| **MaxSpeed** | Fastest roll. Code default is `1`. The prefab stores `0.88`. The spawner multiplies this. |
| **DamageMultiplier** | Scales the hit. Code default is `10`. The prefab stores `300`. |
| **RemoveOnDeployTag** | Entities with this tag are removed when it deploys. Empty on the prefab. |
| **AddOnDeployTag** | Entities with this tag are added when it deploys. Empty on the prefab. The spawner copies its own value here. |
| **PilotStandingPointTag** | Pilot stand tag. The prefab stores `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. The prefab stores `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. The prefab stores `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab stores `batteringram_c_dnm`. |

Pull stands are children tagged `pull`. The hit clips are `batteringram_fire`, `batteringram_fire_weak`, and `batteringram_fire_weakest`. Idle is `batteringram_idle`.

## What it does

Crew on the pull points roll the ram along its path. Speed stays between **MinSpeed** and **MaxSpeed**. At the gate it plays a knock animation and damages the gate. **GateNavMeshId** is the opening the ram is trying to reach. A ditch filler and the bridge faces are chosen by the spawner, not by this prefab.

## Where it lives

Managed script. The weapon prefab is `batteringram_a` in `Native\Prefabs\props_siege_batteringrams.xml`. No scene file stores the weapon script. Siege scenes store the spawner.

## Examples

**Ram prefab.** `batteringram_a` in that file. Ghost motion off, wheel `1.3`, gate navmesh `50`, disabled navmesh `40`, speed `0.25` to `0.88`, damage multiplier `300`, navmesh prefab `batteringram_c_dnm`.
