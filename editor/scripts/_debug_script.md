# _debug_script

!!! info "v1.4.8"

Native debug script. The only opened placement stores a float named **Value**.

Find placed ones with `s:_debug_script`. See [Filter](/editor/editor/#filter).

## Where you put it

The ship connection ramp carries it. No scene file under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## What you set

Native panel. The ramp stores one row.

| Panel field | What to do with it |
|---|---|
| **Value** | Float. The ramp stores `0`. |

## What it does

The ramp keeps **Value** at `0`. The prefab does not show any other effect of that float.

## Where it lives

Native script. The placement is the packed prefab `connection_ramp` in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Ship connection ramp.** Packed prefab `connection_ramp`. The script stores **Value** `0`. A child `ramp` uses mesh `editor_plane_low`.
