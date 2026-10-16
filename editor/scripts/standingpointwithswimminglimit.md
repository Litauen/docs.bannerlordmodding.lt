# StandingPointWithSwimmingLimit

!!! info "v1.4.8"

A [StandingPoint](/editor/scripts/standingpoint/) that only an agent in the water may use. Ship climbers put it on the pilot stand.

Find placed ones in the Scene panel with `s:StandingPointWithSwimmingLimit`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the pilot stand of a climbing machine, tagged `Pilot`. The loose prefabs are `ship_climber` and `longboat_climber_test_fine_tuned` in `NavalDLC\Prefabs\ship_machines.xml`. Packed ship prefabs repeat the same stand.

## What you set

`longboat_climber_test_fine_tuned` stores this order. `ship_climber` writes **NavMeshPrefabName** before the two position fields.

| Panel field | What to do with it |
|---|---|
| **AutoSheathWeapons** | Sheathe on use. Both climbers store on. |
| **AutoEquipWeaponsOnUseStopped** | Equip on leave. Both store off. |
| **AutoWieldWeapons** | Wield on leave. Both store off. |
| **TranslateUser** | Moves the user onto the point. Both store on. |
| **HasRecentlyBeenRechecked** | Runtime flag. Both store off. |
| **_useOwnPositionInsteadOfWorldPosition** | Use the point's own position. Both store on. |
| **_customPlayerInteractionDistance** | Extra player use distance. Both store `3`. |
| **NavMeshPrefabName** | Navmesh prefab. Both leave it empty. |

## What it does

An agent who is not in the water cannot use the point. One who is in the water uses it as a normal standing point. On the climbers this is the pilot stand, with use distance `3`.

## Where it lives

Managed script. `NavalDLC\Prefabs\ship_machines.xml` and `NavalDLC\AssetPackages\nested_prefabs_packed.xml` store it. One scene bakes a climber that includes it.

## Examples

**Ship climber.** `ship_climber` / `mp_spawnpoint`, tagged `pilot`. Sheath on, own position on, interaction distance `3`.

**Longboat climber.** `longboat_climber_test_fine_tuned` / `standing_point`, tagged `Pilot`. Same values. Own position is written before the navmesh row.

**Baked climber.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. `longboat_rope_climber_small_nested` / `standing_point`, tagged `Pilot`. Sheath on, own position on, interaction distance `3`.
