# ChangeLightIntensityScript

!!! info "v1.4.8"

Raises the intensity of the light on this entity after a wait. **Preview** and **Reset** are editor buttons.

Find placed ones in the Scene panel with `s:ChangeLightIntensityScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity that already has a light. The cutscene placement is an empty entity with a light component, intensity starting at `0`.

## What you set

| Panel field | What to do with it |
|---|---|
| **WaitBeforeChangeAsSeconds** | Seconds to wait before the intensity starts moving. Default `0`. |
| **ChangeAmount** | How much intensity to add in total. Default `20`. |
| **ChangeSpeed** | Added to the intensity on each tick until **ChangeAmount** is reached. Default `1`. |
| **Preview** | Starts the wait and the change in the editor. If a change is already running, press **Reset** first. |
| **Reset** | Puts the intensity back to the value it had when the preview started. |

## What it does

On each tick the script reads the light on its own entity. After the wait, it adds **ChangeSpeed** to that intensity until the total added equals **ChangeAmount**, then it stops. It does not loop.

## Where it lives

Managed script. One vanilla scene stores it. No prefab does.

## Examples

**Sister-rescue light that comes up after six seconds.** `NavalDLC\SceneObj\cutscene_saving_sister`. Entity `empty_object` at the wooden platform. The light starts at intensity `0`, radius `11`. The script waits `6` seconds, then adds intensity in steps of `10` until it has added `750`.
