# CheckpointArea

!!! info "v1.4.8"

A box volume. When the player walks inside it, that checkpoint id is saved and the player can be returned to the child spawn point later.

Find placed ones in the Scene panel with `s:CheckpointArea`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity and scale the entity. The box is half the entity scale on each axis around the entity origin. Put a child entity tagged `sp_checkpoint` where the player should reappear.

## What you set

| Panel field | What to do with it |
|---|---|
| **UniqueId** | Number saved when the player enters the box. The reload looks for this same number. Default `0`. |
| **NavMeshPrefabName** | Navmesh prefab to import for this entity. The checkpoint prefab leaves it empty. |

## What it does

While the player is inside the box, the checkpoint mission stores **UniqueId**, if no enemy is cautious, patrolling-cautious, or alarmed. On the next visit, if the saved id matches, the player is moved to the child tagged `sp_checkpoint`.

## Where it lives

Managed script on the volume-box base. The only vanilla copy is the prefab below. No `scene.xscene` under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC contains the script.

## Examples

**Checkpoint box with a spawn child.** `SandBox\Prefabs\sp_editor_spawnpoints.xml`, entity `checkpoint_area`. **UniqueId** is `0`. **NavMeshPrefabName** is empty. The entity itself is tagged `sp_checkpoint`. Its child `sp_checkpoint_player` is tagged `sp_checkpoint` too. The mission uses the child.

The mission that reads the id is the checkpoint mission logic: it searches every `CheckpointArea`, and if none matches it tries `CheckpointUsePoint` with the same id.
