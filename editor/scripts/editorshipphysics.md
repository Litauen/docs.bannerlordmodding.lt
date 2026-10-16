# EditorShipPhysics

!!! info "v1.4.8"

Editor marker stored on a ship hull next to the naval physics. The managed class declares no fields and does not read the two checkboxes the hulls save.

Find placed ones in the Scene panel with `s:EditorShipPhysics`. See [Filter](/editor/editor/#filter).

## Where you put it

It is already on packed ship hulls, on the same entity as `EditorShipUpgradeViewer` and the ship attachment logic. Add it only on a hull you are previewing in the editor.

## What you set

The class body is empty. Every opened placement still stores these two rows, both off. Nothing in the managed script reads them.

| Panel field | What to do with it |
|---|---|
| **Ram Debug Mode** | Checkbox. Packed hulls and both scenes store `false`. |
| **CameraFollowShip** | Checkbox. Same placements store `false`. |

## What it does

The managed class has no init and no tick. Naval physics on a real ship comes from `NavalPhysics` and `MissionShip`, not from this script.

## Where it lives

Managed script. 45 copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. Two scenes store it as well.

## Examples

**Burning cog hull.** Packed entity `burning_cog_ship_nested_storyline`. Both checkboxes are off. `EditorShipUpgradeViewer` is the next script on that entity and stores no variables.

**Funeral boat.** `NavalDLC\SceneObj\scn_cutscene_main_hero_naval_battle_death`. Entity tagged `funeral_boat` (`funeral_boat_nested`). Both checkboxes are off.

**Multi-agent sea conversation.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. One copy, both checkboxes off, on the entity that also has the ship cloth fixer.
