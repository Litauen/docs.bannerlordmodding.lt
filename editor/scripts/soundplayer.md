# SoundPlayer

!!! info "v1.4.8"

Plays one named sound from the entity. **SoundName** is the event id. **AutoStart** plays it when the mission starts. **AutoLoop** starts it again whenever it is not playing.

Find placed ones in the Scene panel with `s:SoundPlayer`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the entity where the sound should sit. The position used is the entity position. No vanilla prefab or scene stores the script.

## What you set

No prefab writes these, so the values are the code defaults.

| Panel field | What to do with it |
|---|---|
| **AutoLoop** | On: if the sound is not playing, it is started again. Default off. |
| **AutoStart** | On: play once when the mission starts. Default off. |
| **SoundName** | Event id. Default empty, which plays nothing. |

## What the player hears

Whatever event **SoundName** names, from this entity. With both checkboxes off and the name empty, the mission starts the script and plays nothing. No vanilla placement stores an event id, so there is no sample id to quote.

## Where the sound lives

Managed script. Event ids live in `Modules\Native\ModuleData\sound_event_data.gen.xml`. This script is not on a prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC.

## Examples

No placed entity. The script is the caller: on mission start it resolves **SoundName** and, when **AutoStart** is on, plays it at the entity.
