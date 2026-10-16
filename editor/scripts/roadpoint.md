# RoadPoint

!!! info "v1.4.8"

An empty marker. It has no panel fields and no vanilla scene places it.

Find placed ones in the Scene panel with `s:RoadPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity if you want the marker in the script list. Nothing in the road script reads these entities.

## What you set

The script adds no fields.

## What it does

The entity stays where you put it. [RoadStart](/editor/scripts/roadstart/) creates its own child mesh and does not look for `RoadPoint` entities.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. There is no other caller. The class only reports that it does not move its entity.
