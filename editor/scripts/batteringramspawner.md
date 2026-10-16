# BatteringRamSpawner

!!! info "v1.4.8"

Spawns a [BatteringRam](/editor/scripts/batteringram/) and tells it which gate, which side, and which path to use.

Find placed ones in the Scene panel with `s:BatteringRamSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `batteringram_a_spawner` from `Native\Prefabs\spawners.xml`. Aim it down the path the ram should follow. The path entity name has to match an entity in the scene.

## What you set

The spawner prefab stores no variables, so these are the code defaults. Siege scenes also store `wait_pos_ground`, a ground frame. That field is hidden from the panel.

| Panel field | What to do with it |
|---|---|
| **SideTag** | Side passed to the ram (`middle`, or a flank tag). Default empty. |
| **GateTag** | Gate tag the ram should hit. Default empty. Scenes use `outer_gate`. |
| **PathEntityName** | Name of the path entity. Default `Path`. |
| **BridgeNavMeshID_1** | Bridge face. Default `8`. |
| **BridgeNavMeshID_2** | Second bridge face. Default `8`. |
| **DitchNavMeshID_1** | Ditch face. Default `9`. |
| **DitchNavMeshID_2** | Second ditch face. Default `10`. |
| **GroundToBridgeNavMeshID_1** | Ground-to-bridge face. Default `12`. |
| **GroundToBridgeNavMeshID_2** | Second ground-to-bridge face. Default `13`. |
| **AddOnDeployTag** | Copied onto the ram. Default empty. |
| **RemoveOnDeployTag** | Copied onto the ram. Default empty. |
| **SpeedModifierFactor** | Multiplies the ram's min and max speed. Default `1`. The editor clamps a change to `0.8`–`1.2`. |
| **EnableAutoGhostMovement** | On: the editor ghost walks **PathEntityName**. Default off. |
| **ToBeSpawnedOverrideName** | Prefab to spawn instead of the default ram. Default empty. |
| **ToBeSpawnedOverrideNameForFireVersion** | Fire-version prefab. Default empty. The ram spawner does not spawn a fire version. |

If the scene has a `ditch_filler` with this **SideTag**, the editor warns when any of those face ids is missing.

## What it does

On mission start it spawns the ram and copies the tags, the path name, the navmesh ids, and the speed multiplier. **SpeedModifierFactor** scales both **MinSpeed** and **MaxSpeed** on the ram. Changing **PathEntityName** retargets the editor ghost.

## Where it lives

Managed script, on the siege-spawner base. Prefab `batteringram_a_spawner` is in `Native\Prefabs\spawners.xml`. Castle and town siege scenes place it.

## Examples

**Bare spawner.** `Native\Prefabs\spawners.xml`, entity `batteringram_a_spawner`. The script is present and no variable is stored.

**Outer gate, level 3 path.** `SandBoxCore\SceneObj\khuzait_castle_siege_001`. A `batteringram_a_spawner` sets **SideTag** to `middle`, **GateTag** to `outer_gate`, and **PathEntityName** to `ram_path_lvl3`. It also stores the hidden ground frame. Bridge and ditch ids stay at the defaults. The entity is on levels `level_2`, `level_3`, and `siege`.
