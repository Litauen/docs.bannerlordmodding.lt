# ItemVisualizer

!!! info "v1.4.8"

An editor tool that lays out a mesh for every item in the game. It has no panel fields, and the meshes are not saved with the scene.

Find placed ones with `s:ItemVisualizer`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to any entity in an editor scene. Placement of that entity does not matter. No official scene or prefab contains it.

## What you set

This script has no panel fields.

## What it does

When the editor finishes loading item data, it walks every item that has a multi-mesh name. Each mesh is a new entity, named with the item name, starting near local position `100, 100` at height `2`. The row wraps after X passes `200`. Those entities are marked not to save. Items with an empty multi-mesh name, or a mesh that fails to load, are skipped.

## Where it lives

There is no prefab and no scene. The meshes come from the loaded item list.

## Examples

No scene or prefab contains `ItemVisualizer`. The layout above is what the script does in the editor.
