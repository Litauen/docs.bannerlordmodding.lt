# RoadStart

!!! info "v1.4.8"

Creates a child entity named `Road_Entity` and assigns a material. Changing the material or the two sweep numbers updates that mesh. No vanilla scene places it.

Find placed ones in the Scene panel with `s:RoadStart`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that should own the road mesh. The child is created when the entity is initialized, including in the editor. Moving this entity refreshes the mesh.

## What you set

| Panel field | What to do with it |
|---|---|
| **textureSweepX** | First sweep value written onto the mesh. Default `0`. |
| **textureSweepY** | Second sweep value. Default `0`. |
| **materialName** | Material resource. Default `blood_decal_terrain_material`. A missing material falls back to the engine default material. |

## What it does

On init it creates `Road_Entity`, builds an empty mesh, and sets **materialName** plus the two sweep numbers. Editing **materialName** rebuilds the mesh. The sweep numbers update the mesh vector without a rebuild. Removing the script removes the child.

It does not read [RoadPoint](/editor/scripts/roadpoint/) entities.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The mesh is created at runtime by this script, under the name `Road_Entity`.
