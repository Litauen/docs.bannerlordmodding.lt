# BoundaryWallView

!!! info "v1.4.8"

Editor-only border mesh. It draws the walk area and the attacker and defender zones from tagged vertices. A mission must not start with this script on an entity.

Find placed ones in the Scene panel with `s:BoundaryWallView`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an editor entity in the scene you are painting. It replaces that entity's meshes while border helpers are on. The vertices it reads are other entities, tagged `walk_area_vertex`, `defender_area_vertex`, or `attacker_area_vertex`. Each tag needs at least three vertices.

## What you set

Nothing. The script declares no visible field.

## What it does

While border helpers are on, the editor rebuilds a wall mesh through those vertices. The walk area is the default border colour. Defender vertices are blue. Attacker vertices are cyan. The mesh uses the material `editor_map_border`. If a tag has fewer than three vertices, that wall is cleared.

Starting a mission with the script on the entity throws. The mission border is drawn by the mission view, which calls the same mesh builder and does not place this script.

## Where it lives

Managed script. No scene and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placement in those modules. The editor tick is the only user of the script. Battle and siege missions draw their border through the mission view instead.
