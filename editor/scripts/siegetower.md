# SiegeTower

!!! info "v1.4.8"

The tower that rolls up to a wall and drops its ramp. Scenes place a [SiegeTowerSpawner](/editor/scripts/siegetowerspawner/), which spawns this prefab. The tower prefab is hidden in the prefab browser.

Find placed ones in the Scene panel with `s:SiegeTower`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the spawner in a siege scene. The tower looks for a path entity, a wall-segment tag, and a child tagged `gate`. Stands tagged `move` are the push points. The gate stand uses Open.

## What you set

`Native\Prefabs\props_siege_siegetowers.xml` stores this order on `siege_tower_5m`. The 9 m and 12 m towers use the same rows with the numbers below.

| Panel field | What to do with it |
|---|---|
| **GateTag** | Gate child. All three store `gate`. |
| **GateOpenTag** | Open-gate child. All three store `gateOpen`. |
| **HandleTag** | Handle child. All three store `handle`. |
| **GateHandleIdleAnimation** | Handle idle. 5 m: `siegetower_5m_door_idle`. 9 m: `siegetower_l2_door_idle`. 12 m: `siegetower_l3_door_idle`. |
| **GateTrembleAnimation** | Ramp stop. 5 m: `siegetower_5m_door_stop`. 9 m: `siegetower_l2_door_stop`. 12 m: `siegetower_l3_door_stop`. |
| **BattlementDestroyedParticle** | Particle when the battlement breaks. 5 m and 12 m store `psys_adobe_battlement_destroyed`. 9 m stores `destroyed`. |
| **GhostEntityMove** | Editor ghost motion. 5 m stores on. 9 m and 12 m store off. |
| **GhostEntitySpeedMultiplier** | Scales that ghost. All three store `1`. |
| **WheelDiameter** | Wheel size while it rolls. 5 m stores `0.74`. 9 m and 12 m store `0.86`. |
| **MinSpeed** | Slowest roll. All three store `0.45`. The spawner multiplies this. |
| **MaxSpeed** | Fastest roll. 5 m stores `0.92`. 9 m stores `0.9`. 12 m stores `0.88`. The spawner multiplies this. |
| **GateNavMeshId** | Navmesh id of the opening at the wall. 5 m stores `0`. 9 m and 12 m store `601`. |
| **NavMeshIdToDisableOnDestination** | Face id turned off on arrival. All three store `-1`. |
| **BarrierTagToRemove** | Default barrier tag on the prefab. All three store `barrier`. The spawner overwrites this. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty on the prefabs. |
| **AddOnDeployTag** | Tag shown on deploy. Empty on the prefabs. |
| **PilotStandingPointTag** | Pilot stand tag. `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. `siege_tower_5m_dnm`, `siege_tower_9m_dnm`, or `siege_tower_12m_dnm`. |

## What it does

Crew on the `move` points push the tower along its path. Speed stays between **MinSpeed** and **MaxSpeed**. At the wall the ramp falls open. The description on a move point is Siege Tower, and the action is Move. The gate point says Gate, and the action is Open.

## Where it lives

Managed script. The three prefabs are in `Native\Prefabs\props_siege_siegetowers.xml`. No scene file stores the weapon script. Siege scenes store the spawner.

## Examples

**5 m tower.** `siege_tower_5m`. Ghost motion on, wheel `0.74`, speed `0.45` to `0.92`, gate navmesh `0`, navmesh prefab `siege_tower_5m_dnm`.

**9 m tower.** `siege_tower_9m`. Ghost motion off, wheel `0.86`, speed `0.45` to `0.9`, gate navmesh `601`, navmesh prefab `siege_tower_9m_dnm`.

**12 m tower.** `siege_tower_12m`. Ghost motion off, wheel `0.86`, speed `0.45` to `0.88`, gate navmesh `601`, navmesh prefab `siege_tower_12m_dnm`.
