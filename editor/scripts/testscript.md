# TestScript

!!! info "v1.4.8"

An editor motion test. The entity spins, slides, and can blink child particles like a water wheel.

Find placed ones with `s:TestScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the entity that should spin or slide. For the water-wheel mode, name the child entities so each name ends in a number. **sideRotatingEntity** and **forwardRotatingEntity** are two extra entities the editor tick rotates.

## What you set

No prefab stores this script. The panel order is the class order.

| Panel field | What to do with it |
|---|---|
| **testString** | Unused string on the panel. |
| **rotationSpeed** | Spin speed about the up axis. |
| **waterSplashPhaseOffset** | Shifts the splash phase. |
| **waterSplashIntervalMultiplier** | Length of each splash window. Code default is `1`. |
| **isWaterMill** | On: children play or pause their particles by the number at the end of the name. |
| **MoveAxisX** | Slide direction X. Code default is `1`. |
| **MoveAxisY** | Slide direction Y. Code default is `0`. |
| **MoveAxisZ** | Slide direction Z. Code default is `0`. |
| **MoveSpeed** | Slide speed. Code default is `0.0001`. |
| **MoveDistance** | How far it travels before it reverses. Code default is `10`. |
| **sideRotatingEntity** | Entity the editor tick rotates about its side axis. |
| **forwardRotatingEntity** | Second entity, rotated more slowly about its side axis. |

## What it does

Every tick the entity rotates about up by **rotationSpeed** and slides along the move axis until **MoveDistance**, then it reverses. With **isWaterMill** on, each numbered child resumes its particles during its slice of the turn and pauses them outside that slice. The two entity slots rotate only in the editor tick.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. There is no other caller. The class itself rotates, slides, and toggles the child particles.
