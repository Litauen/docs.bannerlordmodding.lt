# PopupSceneSequence

!!! info "v1.4.8"

A timer with three states for a notification scene: initial, positive, and negative. The base script only waits. A subclass does the work when the timer fires.

Find placed ones in the Scene panel with `s:PopupSceneSequence`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an entity whose tag is the **name** of a `PopupSceneSpawnPoint` entity. When that spawn point is initialized, it finds every entity with that tag and drives this script.

No vanilla scene stores this base script. The two subclasses are also unplaced. See [PopupSceneSwitchCameraSequence](/editor/scripts/popupsceneswitchcamerasequence/) and [PopupSceneSwitchItemSequence](/editor/scripts/popupsceneswitchitemsequence/).

## What you set

| Panel field | What to do with it |
|---|---|
| **InitialActivationTime** | Seconds in the initial state before `OnInitialState`. Default `0`. |
| **PositiveActivationTime** | Seconds in the positive state before `OnPositiveState`. Default `0`. |
| **NegativeActivationTime** | Seconds in the negative state before `OnNegativeState`. Default `0`. |

The three methods on this class are empty. Put the work on a subclass.

## What it does

The spawn point calls `SetInitialState` when the notification opens, and `SetPositiveState` when the player continues. Each call resets the timer and sets the state. When the matching time is reached, the subclass runs once.

The notification does not call the negative state.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The caller is the notification spawn point: it searches the scene for entities tagged with its own name, then initializes and steps this script with the character visual.
