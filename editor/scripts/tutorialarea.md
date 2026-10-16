# TutorialArea

!!! info "v1.4.8"

One lesson ground in the training field: melee, advanced melee, bow, or mounted. The tag prefix gathers the racks, weapons, and boundaries for that lesson.

Find placed ones with `s:TutorialArea`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an empty entity in the training field. `training_field_2` names that entity `tutorial_area`. Weapons, boundaries, and [TrainingIcon](/editor/scripts/trainingicon/) racks elsewhere in the scene use tags that start with this area's prefix.

## What you set

The melee area in `training_field_2` stores this order.

| Panel field | What to do with it |
|---|---|
| **_typeOfTraining** | `Bow`, `Melee`, `Mounted`, or `AdvancedMelee`. |
| **_tagPrefix** | Tag stem for this lesson. Melee stores `melee_`. |
| **NavMeshPrefabName** | Left empty on the opened area. |

## What it does

The area collects every [TrainingIcon](/editor/scripts/trainingicon/) whose subtype starts with **_tagPrefix**, every weapon tagged with that prefix, and the entity tagged `prefix + boundary`. It can show one lesson's weapons, equip them, mark the racks, and turn the boundaries on or off. A `VolumeBox` on a tagged weapon is the breakable's volume.

## Where it lives

Managed script. Scenes: `Modules\StoryMode\SceneObj\training_field_2` and `training_field_test`.

## Examples

**Melee.** `StoryMode\SceneObj\training_field_2`. Entity `tutorial_area`. Type `Melee`, prefix `melee_`.

**The other three lessons in the same scene.** `AdvancedMelee` with prefix `advmelee_`. `Bow` with prefix `ranged_`. `Mounted` with prefix `mounted_`.

`StoryMode\SceneObj\training_field_test` stores the script as well. The training-field mission reads every `TutorialArea` in the scene.
