# FleePosition

!!! info "v1.4.8"

A point or a line that fleeing troops run toward. **Side** decides which battle side is allowed to use it.

Find placed ones with `s:FleePosition`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an editor-only marker. A single point is one entity. A line is a parent with two or more children: the escape point is the closest spot on the segments between those children. If the entity has no children, the entity position is the only point.

Align it to the terrain. Vanilla markers use `debug_lod_mesh` and are visible only while editing.

An entity tagged `sally_out` is registered only in a sally-out battle. Any other flee marker is always registered.

## What you set

| Panel field | What to do with it |
|---|---|
| **Side** | `both`, `attacker`, or `defender`. Anything else is treated as `both`. |

## What it does

When a soldier or a formation flees, the mission picks the closest usable point on the markers for that side. `both` is available to either side. A mount in a sally-out battle uses the attacker markers.

The `defender` tag on `flee_position` is not what the script reads. **Side** is.

## Where it lives

`Modules\Native\Prefabs\props_other.xml`.

## Examples

**One point, both sides.** Prefab `flee_position`. **Side** `both`. One `debug_lod_mesh`, no children. It also has a `defender` tag.

**A line, both sides.** Prefab `flee_line`. **Side** `both`. Two child `scene_escape_border` markers, the second `5` meters along Y.

**Attacker line and defender line.** Prefabs `flee_line_attacker` (**Side** `attacker`) and `flee_line_defender` (**Side** `defender`).

**Town edges.** `SandBoxCore\SceneObj\khuzait_town_g` places `flee_line` twice: one **Side** `defender`, one **Side** `attacker`. `SandBoxCore\SceneObj\sturgia_town_f` and `SandBoxCore\SceneObj\empire_town_q` do the same split. `SandBoxCore\SceneObj\empire_castle_004` places `flee_line_defender` with **Side** `defender`.

**Interior points.** `SandBox\SceneObj\sturgia_castle_keep_a_l3_interior` places `flee_position` twice: **Side** `attacker` and **Side** `defender`.
