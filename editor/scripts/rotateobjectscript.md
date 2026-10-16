# RotateObjectScript

!!! info "v1.4.8"

Turns the entity around one axis after a wait, then stops at the angle you set. The vanilla use is a dungeon hatch.

Find placed ones in the Scene panel with `s:RotateObjectScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that should turn. The hatch prefab is `cinematic_hatch` in `Modules\NavalDLC\Prefabs\NavalDLC_storyline.xml`. It uses the closed cell-bar mesh `aserai_dungeon_cell_bars_closed`.

## What you set

| Panel field | What to do with it |
|---|---|
| **RotationAxis** | `X`, `Y`, or `Z`. Before the turn, any of those three is treated as `X`, the entity’s side. The hatch stores `X`. |
| **WaitBeforeRotateAsSeconds** | Delay before the turn. Default `2`. The hatch stores `3`. |
| **RotateAngle** | Degrees to turn. The sign picks the direction. The hatch stores `90`. |
| **RotationSpeed** | Degrees per second. Negative turns the other way. The hatch stores `-45`. |
| **PreviewRotateObject** | Editor button. Plays the wait and the turn from the current frame. |
| **StopMovement** | Editor button. Puts the entity back on the frame from the start of the preview. |

The buttons are not saved.

## What it does

After the wait, the entity turns at **RotationSpeed** until it has covered **RotateAngle**. The hatch waits 3 seconds, then turns 90 degrees at 45 degrees per second against the positive direction.

## Where it lives

Managed script. Prefab: `Modules\NavalDLC\Prefabs\NavalDLC_storyline.xml`.

## Examples

**Hatch on the prefab.** `cinematic_hatch`. Axis `X`, wait `3`, angle `90`, speed `-45`.

**The same hatch in the sister rescue.** `NavalDLC\SceneObj\cutscene_saving_sister`. Prefab `cinematic_hatch`, scaled to about `0.55 × 0.91 × 0.39`. The scene sets the wait to `5.9` and the speed to `-50`. Axis and angle stay on the prefab.
