# UsableGameObjectGroup

!!! info "v1.4.8"

A parent marker whose real usable object is a focusable child. The group itself has no panel fields.

Find placed ones with `s:UsableGameObjectGroup`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to a parent entity when the siege-weapon lookup should use a child instead of the parent. The child that matters is the first focusable script under that parent.

## What you set

The script adds no fields. `IsVisible` on the class is the entity visibility, and it is not a panel row.

## What it does

When the siege-weapon list asks what a script is, a group answers with the type of the first focusable script on its children. The parent stays a container.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The siege-weapon lookup is the caller that was opened: a group answers with the type of the first focusable script on its children.
