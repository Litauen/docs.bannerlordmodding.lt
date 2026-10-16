# TrajectoryVisualizer

!!! info "v1.4.8"

Draws the arc of a siege engine. The editor lists the class name `TrajectoryVisualizer`. **ShowTrajectory** is the checkbox.

Find placed ones in the Scene panel with `s:TrajectoryVisualizer`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a siege spawner. The prefabs in `Modules\Native\Prefabs\spawners.xml` put it on the trebuchet, mangonel, and ballista spawners, next to the spawner script.

## What you set

| Panel field | What to do with it |
|---|---|
| **ShowTrajectory** | On: show the arc mesh. The spawner prefabs do not write it, so it stays off. |

The arc uses shot speed, vertical limits, horizontal spread, and air friction. The siege script fills those. Turning the checkbox on before that fill has a volume to draw does nothing visible.

## What it does

With **ShowTrajectory** on and the shot numbers filled, a child mesh shows the volume and is parented to this entity. The mesh is editor-only and is not saved into the scene. Turning the checkbox off hides it. Removing the script removes the mesh.

## Where it lives

Managed script. Prefabs: `Modules\Native\Prefabs\spawners.xml`. Scenes that drop those spawners inherit the script.

## Examples

**Trebuchet spawner.** `Native\Prefabs\spawners.xml`, entity `trebuchet_a_spawner`. Scripts are `TrebuchetSpawner` and `TrajectoryVisualizer`. No variables on the visualizer.

**Mangonel spawner.** Same file, entity `mangonel_a_spawner`. `MangonelSpawner` then `TrajectoryVisualizer`, again with no variables. `mangonel_b_spawner` matches that.

**Ballista spawners.** Same file, `ballista_a_spawner` and `ballista_b_spawner`. Each is `BallistaSpawner` plus `TrajectoryVisualizer` with no variables.
