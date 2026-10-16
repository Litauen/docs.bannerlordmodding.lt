# DestructableMissionObject

!!! info "v1.4.8"

Old destruction script. Do not add it. A mission that still has one on an entity closes the game when that entity initialises.

Find placed ones in the Scene panel with `s:DestructableMissionObject`. See [Filter](/editor/editor/#filter).

## Where you put it

Nowhere. The replacement is `DestructableComponent`.

## What you set

The class adds no fields.

## What it does

In the editor, opening a scene that still has this script reports that the scene is on the old destruction prefabs. When a mission initialises the entity, the game exits.

## Where it lives

Managed script, marked obsolete. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

**No placed copy.** The only behavior in the class is that editor init and mission init both reject the old prefab. Nothing in the current official scenes calls it.
