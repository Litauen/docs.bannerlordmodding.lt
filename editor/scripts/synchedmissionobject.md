# SynchedMissionObject

!!! info "v1.4.8"

Marks an entity so its transform, animation, body flags, and colors stay in step across the network.

Find placed ones with `s:SynchedMissionObject`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to a prop that multiplayer has to share. Doors, siege parts, and damage decals use it. Many other scripts already derive from it, so those entities do not need a second copy.

## What you set

`damage_decal_stone` stores the only row.

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh imported for this entity. The decal stores an empty string. |

## What it does

The entity keeps one shared frame. Position, rotation, a playing animation, body flags, and the two colors are what get copied to other clients. An empty **NavMeshPrefabName** imports nothing.

## Where it lives

Managed script. One prefab that stores it alone is `damage_decal_stone` in `Modules\Native\Prefabs\props_other.xml`.

## Examples

**Stone damage decal.** `damage_decal_stone`. Tagged `damage_decal`. **NavMeshPrefabName** empty.

The same script tag is also stored in Native multiplayer scenes, `SandBox\SceneObj\empire_town_f`, SandBoxCore towns, and the NavalDLC packed ship prefabs.
