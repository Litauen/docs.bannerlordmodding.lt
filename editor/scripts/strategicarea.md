# StrategicArea

!!! info "v1.4.8"

An archer standing area. The side on the script decides who may use it.

Find placed ones with `s:StrategicArea`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an empty marker, usually hidden outside the editor. `strategic_archer_point` is the defender marker. `strategic_attacker_archer_position` is the attacker marker. Both are in `Modules\Native\Prefabs\props_siege.xml`. Width scales the marker.

## What you set

`strategic_archer_point` stores this order. **_disableShimmy** and **NavMeshPrefabName** are panel fields that prefab does not write.

| Panel field | What to do with it |
|---|---|
| **_width** | Width of the area. Both prefabs store `1`. |
| **_side** | Who may use it. Defender prefab: `Defender`. Attacker prefab: `Attacker`. |
| **_depth** | Depth. Both prefabs store `1`. |
| **_distanceToCheck** | How far the area looks. Defender prefab: `40`. Attacker prefab: `60`. Code default is `10`. |
| **_ignoreHeight** | Skip height in that check. Both prefabs store on. |
| **_disableShimmy** | Stop the side-step. Code default is off. The prefabs do not write this row. |
| **NavMeshPrefabName** | Left empty. The prefabs do not write this row. |

## What it does

Each battle side collects its areas and sends archers there. **_side** is the filter. **_distanceToCheck** is the look distance, and **_ignoreHeight** drops the height part of that test. With **_disableShimmy** off, the men step around the point.

## Where it lives

Managed script. Prefabs: `Modules\Native\Prefabs\props_siege.xml`.

## Examples

**Defender archers.** `strategic_archer_point`. **_side** `Defender`, **_distanceToCheck** `40`.

**Attacker archers.** `strategic_attacker_archer_position`. **_side** `Attacker`, **_distanceToCheck** `60`.

**Villages and towns.** `SandBox\SceneObj` raid scenes and `SandBoxCore` castle scenes store the script on their siege layouts. `NavalDLC\SceneObj\nord_castle_a` stores it too.
