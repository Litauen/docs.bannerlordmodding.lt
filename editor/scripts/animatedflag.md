# AnimatedFlag

!!! info "v1.4.8"

Waves the entity's first mesh as that entity moves. The script has no panel fields.

Find placed ones in the Scene panel with `s:AnimatedFlag`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an entity that already has a mesh. Vanilla uses the mesh `banner_test`.

## What you set

Nothing. The script declares no visible field, and the prefabs store no variables.

## What it does

Each tick it yaws and skews that first mesh from how far the entity moved since the last tick. It is visual only.

## Where it lives

Managed script. No scene under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it. The placements are prefabs.

## Examples

**Test banner.** `Native\Prefabs\editor_helpers_&_tests.xml`, entity `banner_simple_test`, mesh `banner_test`. The script is on the entity with no variables.

**Jousting banners.** `SandBox\Prefabs\sp_editor_spawnpoints.xml`. `jousting_banner_0` is tagged `banner_0` and scaled about `2.25`. `jousting_banner_1` is tagged `banner_1` and scaled about `1.69`. Both use mesh `banner_test` and the script with no variables.
