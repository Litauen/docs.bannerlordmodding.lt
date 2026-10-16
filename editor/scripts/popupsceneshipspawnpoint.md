# PopupSceneShipSpawnPoint

!!! info "v1.4.8"

A marker the naval notification looks up before it parents a visual ship to that entity. The script has no panel fields.

Find placed ones in the Scene panel with `s:PopupSceneShipSpawnPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that should receive the ship. Tag that entity `spawnpoint_ship_1`, `spawnpoint_ship_2`, and so on, in the order the notification lists its ships.

## What you set

The script adds no fields.

## What it does

When a notification has ships, it looks up `spawnpoint_ship_` plus the ship number, deletes that entity’s children, and parents a visual ship built from the notification’s ship prefab, upgrades, seed, hit points, and sail colors.

The lookup of this script’s type is not used after it is found. The tag and the entity are what place the ship. A missing tag skips that ship.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The caller is the scene notification. It asks the notification data for its ship list and spawns each one under `spawnpoint_ship_1`, then `spawnpoint_ship_2`, and so on.

The naval death scene `scn_cutscene_main_hero_naval_battle_death` does not use those tags. Its boat is an entity tagged `funeral_boat`, driven by [PopupSceneShipController](/editor/scripts/popupsceneshipcontroller/).
