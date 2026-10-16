# EditorShipUpgradeViewer

!!! info "v1.4.8"

Empty editor script placed on a ship hull beside `EditorShipPhysics`. It adds no fields and no managed behavior.

Find placed ones in the Scene panel with `s:EditorShipUpgradeViewer`. See [Filter](/editor/editor/#filter).

## Where you put it

On the hull entity, with the other editor ship scripts. The packed burning cog puts it immediately after `EditorShipPhysics`.

## What you set

No panel fields. Opened copies store no variables.

## What it does

The managed class is empty. It does not change upgrade level, meshes, or physics.

## Where it lives

Managed script. The copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml` (43 of them). No `scene.xscene` under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores the script itself.

## Examples

**Burning cog hull.** Packed entity `burning_cog_ship_nested_storyline`. `EditorShipUpgradeViewer` has no variables. The script before it on that entity is `EditorShipPhysics`, which stores **Ram Debug Mode** and **CameraFollowShip**, both off.
