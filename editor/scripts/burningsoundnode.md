# BurningSoundNode

!!! info "v1.4.8"

Plays the ship-fire bed from this entity. Nearby [BurningNode](/editor/scripts/burningnode/) points drive how loud the fire is. The script has no panel fields.

Find placed ones in the Scene panel with `s:BurningSoundNode`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the packed prefab `burning_sound_node`, and place copies where a stretch of fire should be heard. The ship fire system gathers every copy under the ship.

## What you set

Nothing. The script declares no visible field, and `burning_sound_node` stores no variables.

## What the player hears

The clip is `event:/mission/ambient/detail/fire/fire_dynamic`. On that sound, `length="-1"` keeps it going until the fire stops, and `max_distance="25"` keeps it local. The parameter it accepts is `FireIntensity`.

The fire system starts the sound. Intensity is the sum of fire amounts on burning nodes within `5` meters of this entity. Stopping the fire stops the sound and clears that list.

## Where it lives

Managed script. The sound row is in `Native\ModuleData\sound_event_data.gen.xml`. The prefab is `burning_sound_node` in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No loose scene stores the script.

## Examples

**Fire bed.** That packed prefab, entity `burning_sound_node`. The script is present and has no variables. The same packed file has further copies of the script.
