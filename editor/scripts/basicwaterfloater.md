# BasicWaterFloater

!!! info "v1.4.8"

Pins the entity's height to the water surface at its position. The script has no panel fields.

Find placed ones in the Scene panel with `s:BasicWaterFloater`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the entity that should sit on the water. XY stays where you put the entity. Z is rewritten every tick.

## What you set

Nothing. The script declares no visible field.

## What it does

In the editor and in a mission, each tick sets the entity's Z to the water level at its XY. It does not move the entity sideways and it does not tilt it.

## Where it lives

Managed script. No scene and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it. No other vanilla class adds it at runtime.

## Examples

No placement in those modules. The script only runs the height pin described above.
