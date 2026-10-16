# CinematicBurningArrow

!!! info "v1.4.8"

Fires this entity along its forward axis like a falling arrow, and plays the alert-arrow sound. The entity starts hidden until a shot begins.

Find placed ones in the Scene panel with `s:CinematicBurningArrow`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the arrow entity. Aim the entity: forward is the launch direction. Hideout ambushes put it on `arrow_root`, tagged `hideout_ambush_cutscene_arrow_path`.

## What you set

| Panel field | What to do with it |
|---|---|
| **_speed** | Launch speed along the entity forward. Default `10`. The ambush prefab stores `21`. |
| **ShootArrow** | Editor button. Puts the entity back on its start frame, then launches it. |
| **StopMovement** | Editor button. Stops the shot and puts the entity back on its start frame. |

## What it does

A shot shows the entity, plays `event:/mission/ambient/special/alert_arrow`, and moves it with gravity. The shot ends when **_speed** is `0` or when four seconds have passed. The entity is hidden again and the sound stops. In the editor, while the shot is idle, the script samples the arc from the current frame.

## Where it lives

Managed script. The shared values sit on entity `arrow_root` inside `SandBox\Prefabs\sp_editor_spawnpoints.xml`.

## Examples

**Forest hideout ambush arrow.** `SandBox\SceneObj\forest_hideout_004_sv`. Entity `arrow_root`, tag `hideout_ambush_cutscene_arrow_path`, **_speed** `21`.

**Sea-bandit ambush arrow.** `SandBox\SceneObj\sea_bandit_a_sv`. The same entity name, tag, and speed.
