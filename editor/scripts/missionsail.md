# MissionSail

!!! info "v1.4.8"

The sail on a ship. Wind pushes through it, and the sheet trims and folds with the ship's sail setting. The script adds no fields of its own.

Find placed ones with `s:MissionSail`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the sail entity, next to `sail_visual`. Every descendant of that entity, including the sail itself, must have **Does not Affect Parent's Local Bounding Box** set. If one is missing, the editor warns on the sail. The cloth, the top banner, and `force_center_entity` live under that sail.

Vanilla keeps a single authored copy on the packed `sail_holder` prefab. Ships reference that sail.

## What you set

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh for this entity. The sail stores it empty. |

## What it does

The sail takes its size from the physics box and its type from the ship sail data. It rotates toward the wind, builds force from the wind relative to the ship, and folds or opens with the sail setting. A full sheet and a gust change how hard that force pushes. Hits reduce sail health. The sail can burn, and a capture can swap its texture.

Two sounds follow the sail: `event:/mission/movement/vessel/sail/sail_movement` and `event:/mission/movement/vessel/sail/sail_rotation`.

## Where it lives

`Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`, prefab `sail_holder`.

## Examples

No scene file stores this script.

**Square sail.** Prefab `sail_holder`, entity `sail_square_medium_nested`. `MissionSail` stores an empty **NavMeshPrefabName**. `sail_visual` on the same entity stores **Top Flag Rope Position** `0.8` and **Capture Flag Bottom Rope Position** `0.25`. Child `force_center_entity` marks the force point.
