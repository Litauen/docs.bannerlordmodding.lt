# HandPose

!!! info "v1.4.8"

An editor pose that drives the entity skeleton to the hand-armor tableau pose. It has no panel fields.

Find placed ones with `s:HandPose`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity that already has a `human_skeleton`. The script copies the entity components onto that skeleton and freezes it in the hand pose. Vanilla uses an editor-only entity.

## What you set

This script has no panel fields.

## What it does

Once the editor has game data, the skeleton plays `act_tableau_hand_armor_pose` on channel 0 and stays frozen there. Later editor ticks keep that pose. It does not run as mission logic.

## Where it lives

`Modules\Native\SceneObj\scn_item_tableau`.

## Examples

**Glove tableau.** `Native\SceneObj\scn_item_tableau`, entity `glove_frame`, tag `glove_frame`, visible only while editing. It has a `human_skeleton` and a `HandPose` script with no variables. A child `glove_cam` (tag `glove_cam`) sits beside it.
