# terrain_mesh_exporter

!!! info "v1.4.8"

Writes the terrain mesh out of the editor. **Generate** is the button. No official scene or prefab stores the script.

Find placed ones in the Scene panel with `s:terrain_mesh_exporter`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity in the scene you want to export. Nothing in Native, SandBox, SandBoxCore, StoryMode, or NavalDLC places it.

## What you set

Native panel.

| Panel field | What to do with it |
|---|---|
| **Generate** | Writes the mesh. The output folder is `./exported_terrain_meshes/`, including `albedo.png`. |

## What it does

**Generate** exports the terrain mesh and an albedo image into `./exported_terrain_meshes/`. There is no saved placement to show a default.

## Where it lives

Native script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity.
