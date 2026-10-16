# SceneLeveler

!!! info "v1.4.8"

Editor tool that copies a selection set into another level by swapping `_l1`, `_l2`, and `_l3` in the prefab name.

Find placed ones in the Scene panel with `s:SceneLeveler`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop the prefab `scene_leveler` from `Modules\Native\Prefabs\editor_helpers_&_tests.xml` in the scene, fill the two set names, and use the buttons. The entity does nothing during play.

## What you set

| Panel field | What to do with it |
|---|---|
| **SourceSelectionSetName** | Selection set to read. Empty names are rejected. |
| **TargetSelectionSetName** | Selection set the new entities are put in. |
| **CreateLevel1** | Button. Spawns the `_l1` prefab for each source entity that has a level mask. |
| **CreateLevel2** | Button. Same for `_l2`. |
| **CreateLevel3** | Button. Same for `_l3`. |
| **DeleteLevel1** | Button. Deletes entities that are only level 1, and clears the level-1 bit on the rest. |
| **DeleteLevel2** | Button. Same for level 2. |
| **DeleteLevel3** | Button. Same for level 3. |
| **SelectEntitiesWithoutLevel** | Button. Selects every entity in the scene that has no level mask. |

A source entity must already carry level 1, 2, or 3. Its prefab name must change when `_l1`, `_l2`, or `_l3` is swapped in, or that entity is skipped. `WallSegment` and `mesh_bender` are copied onto the new entity when the source has them and the new one does not.

The helper prefab leaves both names empty. The buttons are not saved.

## What it does

**CreateLevel** instantiates the renamed prefab on the source entity’s frame, sets that level bit, and selects the results under **TargetSelectionSetName**. Entities that already have the bit are selected and not duplicated.

## Where it lives

Managed script. Prefab: `Modules\Native\Prefabs\editor_helpers_&_tests.xml` (`scene_leveler`).

## Examples

**Empty helper.** Prefab `scene_leveler`. Both set names are empty.

**Town, level 3 down to level 2.** `SandBoxCore\SceneObj\aserai_town_a`. Prefab `scene_leveler`. **SourceSelectionSetName** `lvl3`, **TargetSelectionSetName** `lvl2`. The entity is on levels 1, 2, 3, civilian, siege, and sally.

**Nord map icon, civilian set.** `NavalDLC\Prefabs\nord_map_icons.xml`. Prefab `town_NX_Y`, child entity `n1_civilian`. Source `nord1`, target `nord3`. `nord_castle_a` and `nord_castle_b` in that file store the same pair on a child of the same name.
