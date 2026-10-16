# MultiplayerBatteringRamSpawner

!!! info "v1.4.8"

Spawns the multiplayer battering ram and aims it at a gate along a path. The spawned ram has `12000` hit points. **SpeedModifierFactor** still scales its speed.

Find placed ones in the Scene panel with `s:MultiplayerBatteringRamSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use prefab `batteringram_a_spawnerMP` from `Modules\Native\Prefabs\spawners.xml`. Empty **ToBeSpawnedOverrideName** spawns `batteringram_a` (entity name minus the last `_piece`). Put a path entity in the scene and set **PathEntityName** to that path's name. **GateTag** is the gate this ram belongs to. **SideTag** is the wall side (`middle` on the siege map below).

## What you set

Prefab order, after the hidden ground frame. The editor clamps **SpeedModifierFactor** to `0.8`–`1.2`.

| Panel field | What to do with it |
|---|---|
| **SideTag** | Wall side. Prefab empty. The siege map uses `middle`. |
| **GateTag** | Gate this ram drives at. Prefab empty. The siege map uses `outer_gate`. |
| **PathEntityName** | Path the ram follows. Prefab `Path`. The siege map uses `ram`. |
| **BridgeNavMeshID_1** | Navmesh id on the bridge. Prefab `8`. |
| **BridgeNavMeshID_2** | Second bridge id. Prefab `8`. |
| **DitchNavMeshID_1** | Ditch id. Prefab `9`. |
| **DitchNavMeshID_2** | Second ditch id. Prefab `10`. |
| **GroundToBridgeNavMeshID_1** | Ground-to-bridge id. Prefab `12`. |
| **GroundToBridgeNavMeshID_2** | Second ground-to-bridge id. Prefab `13`. |
| **AddOnDeployTag** | Tag added when the ram deploys. Empty. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty. |
| **SpeedModifierFactor** | Multiplies min and max speed. Prefab `1`. The siege map uses `1.2`. |
| **EnableAutoGhostMovement** | Editor ghost walks **PathEntityName**. Prefab off. |
| **ToBeSpawnedOverrideName** | Prefab override. Empty uses `batteringram_a`. |
| **ToBeSpawnedOverrideNameForFireVersion** | Unused here. This spawner has no fire version. |

The prefab also saves **wait_pos_ground**. That row is hidden. Move the ground-wait ghost; the matrix is what gets stored.

## What it does

Spawns `batteringram_a`, sets its hit points to `12000`, scales speed by **SpeedModifierFactor**, and passes the gate, side, path, and navmesh ids through to the ram.

## Where it lives

Managed script. Prefab `batteringram_a_spawnerMP` in `Modules\Native\Prefabs\spawners.xml`. Used on the multiplayer siege maps under `Modules\Native\SceneObj`.

## Examples

**Outer gate, faster ram.** `Native\SceneObj\mp_siege_map_003`. One `batteringram_a_spawnerMP`, tag `mp_siege_objective_battering_ram`. **SideTag** `middle`, **GateTag** `outer_gate`, **PathEntityName** `ram`, **SpeedModifierFactor** `1.2`.
