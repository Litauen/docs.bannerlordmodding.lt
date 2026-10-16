# StealthIndoorLightingArea

!!! info "v1.4.8"

A volume that replaces outdoor light levels for stealth vision. Scale the entity to the room. Agents inside are judged with **AmbientLightStrength** and **SunMoonLightStrength**.

Find placed ones in the Scene panel with `s:StealthIndoorLightingArea`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `stealth_indoor_lighting_area` from `SandBox\Prefabs\lighting.xml`. The mesh is `editor_cube`, visible only in the editor. Scale it over the interior.

## What you set

That prefab stores this order.

| Panel field | What to do with it |
|---|---|
| **AmbientLightStrength** | Ambient level used while an agent is inside. The prefab stores `0`. |
| **SunMoonLightStrength** | Sun or moon level used while an agent is inside. The prefab stores `0`. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab leaves it empty. |

## What it does

Sentries rate how well they see an agent. Outside these volumes the daytime pair is ambient `0.7` and sun `1`, and the nighttime pair is ambient `0.2` and moon `0.15`. The first volume that contains the agent replaces both numbers with the two fields. A disguise mission collects every entity with this script when the mission starts.

## Where it lives

Managed script, on the volume-box base. The prefab is `stealth_indoor_lighting_area` in `SandBox\Prefabs\lighting.xml`. No scene file stores the script.

## Examples

**Dark interior volume.** That prefab. Both strengths `0`, navmesh empty, editor-only cube. No scene override was stored.
