# StealthAreaUsePoint

!!! info "v1.4.8"

The camp point that calls hideout reinforcements. It stays dark until the sentry markers under it are clear, and it stays closed while anyone is alarmed.

Find placed ones in the Scene panel with `s:StealthAreaUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `stealth_area_use_point` from `SandBox\Prefabs\sp_editor_spawnpoints.xml`. Child [StealthAreaMarker](/editor/scripts/stealthareamarker/) entities define the pockets. The prefab's campfire is a child. A nested child named `highlight_pointer_glow_ground` is the glow that hides after use.

## What you set

The prefab stores this order. The forest hideout keeps the same values.

| Panel field | What to do with it |
|---|---|
| **ActionStringId** | Text id for the action line. Empty uses `str_call_troops`. The prefab and the forest scene store empty. |
| **DescriptionStringId** | Text id for the description. Empty leaves the description blank. Both store empty. |
| **NavMeshPrefabName** | Navmesh prefab. Both leave it empty. |

## What it does

The player can use it when it is enabled, it has not been used yet, and no agent is alarmed. Use plays `event:/mission/combat/pickup_arrows`, hides the glow, and pauses AI agents. When every sentry marker under this point is empty, the point enables and plays `event:/ui/notification/quest_update`. The non-ambush hideout controller turns these points off.

## Where it lives

Managed script. Prefab `stealth_area_use_point` is in `SandBox\Prefabs\sp_editor_spawnpoints.xml`. Hideout scenes under `SandBox\SceneObj` place it.

## Examples

**Prefab camp.** `stealth_area_use_point`. Both text ids empty. A `campfire` child holds the fire mesh. Marker children sit further down the same prefab.

**Forest hideout.** `SandBox\SceneObj\bandit_forest_sv`. One `stealth_area_use_point` at about `541, 439, 37`. Text ids empty. Marker children store radii `45` and `40`.

**Sea hideout.** `SandBox\SceneObj\sea_bandit_a_sv` places the same prefab. Its markers include radii `35`, `90`, and `60`.
