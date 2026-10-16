# TrainingIcon

!!! info "v1.4.8"

A weapon rack the player selects in the training field. The description and the subtype tag decide which lesson it belongs to.

Find placed ones with `s:TrainingIcon`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `training_weapon_rack_a` from `Modules\Native\Prefabs\quest_props.xml`, or place that rack in the training field. A child tagged `highlight_beam` is the marker. Other children with no script are the weapon meshes the rack shows and hides.

## What you set

The bow rack stores this order.

| Panel field | What to do with it |
|---|---|
| **_descriptionTextOfIcon** | Text id under `str_tutorial_`. The bow rack stores `bow_training_description`. |
| **_trainingSubTypeTag** | Lesson tag. The bow rack stores `ranged_bow`. A training-field rack stores `ranged_crossbow`. |
| **PilotStandingPointTag** | Use point tag. Prefab: `Pilot`. |
| **AmmoPickUpTag** | Prefab: `ammopickup`. |
| **WaitStandingPointTag** | Prefab: `Wait`. |
| **NavMeshPrefabName** | Left empty. |

## What it does

The use prompt is Select. The description is the tutorial text for **_descriptionTextOfIcon**. Picking the rack up marks it active. The training area collects every rack whose **_trainingSubTypeTag** starts with that area's prefix, then shows the beam and the weapon meshes for the active one.

## Where it lives

Managed script. Prefab: `Modules\Native\Prefabs\quest_props.xml`.

## Examples

**Bow rack.** `training_weapon_rack_a`. Description `bow_training_description`, tag `ranged_bow`.

**Crossbow rack in the lesson.** `StoryMode\SceneObj\training_field_2`. Description `crossbow_training_description`, tag `ranged_crossbow`, with a child `highlight_beam_white` tagged `highlight_beam`.

`StoryMode\SceneObj\training_field_test` stores the script as well.
