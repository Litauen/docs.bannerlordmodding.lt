# WeaponSpawner

!!! info "v1.4.8"

Spawns the item named by the entity, with physics, and moves the entity's tags onto that item.

Find placed ones with `s:WeaponSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Name the entity with the item id (`western_javelin_1_t2`, `bodkin_arrows_a`). Put the category tag on that same entity (`ranged_bow`, `melee_sword`). The script has no mesh of its own. The training-field frames are empty entities.

## What you set

Nothing. The script declares no visible field.

## What it does

Spawn removes the entity's children, spawns that item id at the entity frame with physics, copies the tags onto the new item, and clears them from the spawner entity. The training-field tutorial calls spawn when an entity has this script and a tag that starts with the tutorial prefix.

## Where it lives

Managed script. The bow frame is `Modules\Native\Prefabs\quest_props.xml`. The training frames are in the training-field scenes.

## Examples

**Bow frame.** `quest_props.xml`, entity `weapon_spawner_bow`, tag `ranged_bow`. Empty script.

**Javelin on the training field.** `StoryMode\SceneObj\training_field_2`. Entity `western_javelin_1_t2`, tag `ranged_javelin`.

**Bow, sword, and shield.** `StoryMode\SceneObj\training_field_test`. `bodkin_arrows_a` is tagged `ranged_bow`. `long_tournament_sword` is tagged `melee_sword`. `reinforced_horsemans_kite_shield` is tagged `melee_shield`. A second sword and shield pair uses `advmelee_sword` and `advmelee_shield`.
