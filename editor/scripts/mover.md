# Mover

!!! info "v1.4.8"

Moves this entity along a named scene path during a battle. No vanilla scene or prefab stores the script.

Find placed ones in the Scene panel with `s:Mover`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that should travel. The path is a scene path, not a child of this entity. The entity has to be visible or it does not tick in a mission.

## What you set

| Panel field | What to do with it |
|---|---|
| **_pathname** | Name of the scene path. Empty does nothing. |
| **_speed** | Meters per second along that path. Default `0`, so the entity stays on the first point. |
| **_moveGhost** | Editor only. On: a faint copy walks the path. Off: that copy is hidden. |

## What it does

In the editor, a ghost copy sits on the path while **_pathname** names a real path. In a battle, while the entity is visible and has not reached the end of the path, it advances by **_speed** and takes the path frame. It does not loop in the mission. The editor ghost does loop while **_moveGhost** is on.

## Where it lives

Managed script. Nothing under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC scenes or prefabs writes it. No other script creates it.

## Examples

No placement. The behavior above is the script itself: path name, speed, and the editor ghost.
