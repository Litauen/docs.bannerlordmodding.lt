# FlagCapturePoint

!!! info "v1.4.8"

A siege flag that slides between a raised point and a lowered point. The flag index picks which extra entities follow that flag's team colors and visibility.

Find placed ones with `s:FlagCapturePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the flag pole. The pole needs four tagged children:

- `score_stand` holds the moving flag.
- `flag_white` is the cloth on that stand.
- `flag_raising_top` is the raised position.
- `flag_raising_bottom` is the lowered position.

Anything else in the scene that should hide or recolor with this flag gets the tag `depends_flag_` plus the index (`depends_flag_0`, `depends_flag_1`, and so on).

While the pole is selected, the editor draws two circles on the ground: radius `4`, and a wider contested ring at `6`.

## What you set

| Panel field | What to do with it |
|---|---|
| **FlagIndex** | Which flag this pole is. `0` is letter A. Vanilla poles use `0` through `6`. |
| **NavMeshPrefabName** | Navmesh for this entity. The flag prefabs leave it empty. |

## What it does

On mission start the flag stand is snapped to `flag_raising_top`. A full trip between top and bottom takes 10 seconds at speed 1. Capture logic elsewhere moves the stand up or down and paints `flag_white` with the team colors. Entities tagged `depends_flag_` plus this index get the same colors and the same visibility. Removing the point hides the pole and those dependents.

## Where it lives

`Modules\Native\Prefabs\archhitecture_aserai.xml`.

## Examples

**Seven sergeant poles.** That prefab file defines `flag_pole_big_sergeant_A` through `F` with **FlagIndex** `0` through `5`, and `flag_pole_big_sergeant_main` with **FlagIndex** `6`. Each has `score_stand`, `flag_white`, `flag_raising_top` (about `6.07` up), and `flag_raising_bottom` (about `2.13` up). **NavMeshPrefabName** is empty.

**Siege map, same indexes.** `Native\SceneObj\mp_siege_map_010`. The scene stores the poles themselves (not a prefab reference): `flag_pole_big_sergeant_A` at index `0`, `B` at `1`, `C` at `2`, `D` at `3`, `E` at `4`, `F` at `5`, and `flag_pole_big_sergeant_main` at `6`.
