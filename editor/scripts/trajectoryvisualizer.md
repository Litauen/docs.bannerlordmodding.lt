# TrajectoryVisualizer

!!! info "v1.4.8"

An editor arc for a siege engine. Turn it on to see the shot volume while you place the spawner.

Find placed ones with `s:TrajectoryVisualizer`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the siege spawner, next to the spawner script. `trebuchet_a_spawner` carries it beside `TrebuchetSpawner`. `Modules\Native\Prefabs\props_siege.xml` stores it as well.

## What you set

The opened block in `props_siege.xml` stores the only row.

| Panel field | What to do with it |
|---|---|
| **ShowTrajectory** | On: draw the arc. The prefab stores off. |

## What it does

**ShowTrajectory** draws a trajectory mesh on this entity in the editor when the speed and the angle range are already set. Off, that mesh stays hidden. The mesh is not saved into the scene.

## Where it lives

Managed script. On siege spawners in `Modules\Native\Prefabs\props_siege.xml` and `Modules\Native\Prefabs\spawners.xml`.

## Examples

**Trebuchet marker.** `trebuchet_a_spawner` in `spawners.xml` has the script and stores no variables, so **ShowTrajectory** stays off.

**Mangonel props.** An entity in `props_siege.xml` stores **ShowTrajectory** `false`.

**Siege scenes.** `Native\SceneObj\mp_siege_map_003`, `SandBox\SceneObj\empire_town_f`, and `NavalDLC\SceneObj\nord_castle_b` store the script.
