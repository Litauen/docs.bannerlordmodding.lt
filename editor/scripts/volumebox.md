# VolumeBox

!!! info "v1.4.8"

A box volume. The entity scale is the box. Agents and points inside it are what other scripts ask about.

Find placed ones with `s:VolumeBox`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity with the `volume_box` mesh, tagged `volumebox`, and scale the entity to the size you want. A stone-pile child in `props_siege.xml` is scaled to `5 × 5 × 3`. Training, stealth, and siege scripts find these by tag.

## What you set

The stone-pile child stores the only row.

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh for this box. The opened child stores an empty string. |

## What it does

A point is inside when it falls within half the entity scale on each axis. Stone piles pair a box with a throwing point by tag and wait while an enemy is inside. Tutorial weapons, the training-field checkpoints, the villa sneak triggers, and the naval storyline zones read the same test.

## Where it lives

Managed script. A child in `Modules\Native\Prefabs\props_siege.xml`. Also `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml`.

## Examples

**Stone-pile range.** A child in `props_siege.xml`, tagged `volumebox`, mesh `volume_box`, scale `5, 5, 3`. **NavMeshPrefabName** empty.

**Training checkpoints.** `StoryMode` training-field logic looks up `VolumeBox` on entities tagged `mounted_checkpoint`.

**Sneak lesson.** `StoryMode` villa logic looks up boxes tagged `trigger_volume_crouch`, `trigger_volume_walk_slowly`, and the other `trigger_volume_*` names.

`SandBox\SceneObj\Main_map` and `NavalDLC\SceneObj\Main_map` store the script as well.
