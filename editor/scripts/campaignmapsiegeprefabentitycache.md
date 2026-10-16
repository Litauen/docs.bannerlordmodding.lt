# CampaignMapSiegePrefabEntityCache

!!! info "v1.4.8"

Names the campaign-map icons for each siege engine and side. The map screen reads this once and keeps the launch frame and scale from each icon prefab.

Find placed ones in the Scene panel with `s:CampaignMapSiegePrefabEntityCache`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an empty entity in the campaign map scene. Both the naval map and the land map do that. One copy is enough. The map screen takes the first entity that has the script.

## What you set

Both main maps store this order and these prefab names.

| Panel field | What to do with it |
|---|---|
| **_attackerBallistaPrefab** | Attacker ballista icon. Both maps store `ballista_a_mapicon`. |
| **_defenderBallistaPrefab** | Defender ballista icon. Both maps store `ballista_b_mapicon`. |
| **_attackerFireBallistaPrefab** | Attacker fire ballista. Both maps store `ballista_a_fire_mapicon`. |
| **_defenderFireBallistaPrefab** | Defender fire ballista. Both maps store `ballista_b_fire_mapicon`. |
| **_attackerMangonelPrefab** | Attacker mangonel (onager). Both maps store `mangonel_a_mapicon`. |
| **_defenderMangonelPrefab** | Defender mangonel (catapult). Both maps store `mangonel_b_mapicon`. |
| **_attackerFireMangonelPrefab** | Attacker fire mangonel. Both maps store `mangonel_a_fire_mapicon`. |
| **_defenderFireMangonelPrefab** | Defender fire mangonel. Both maps store `mangonel_b_fire_mapicon`. |
| **_attackerTrebuchetPrefab** | Attacker trebuchet. Both maps store `trebuchet_a_mapicon`. |
| **_defenderTrebuchetPrefab** | Defender trebuchet (bricole). Both maps store `trebuchet_b_mapicon`. |

Each icon prefab needs a first child with a child tagged `projectile_position`. That frame is the launch point the map uses. Leave the names alone unless you have a replacement icon with that tag.

## What it does

When the campaign map starts, each named prefab is spawned, measured, and kept as the icon for that engine and side. Attacker ballista, fire ballista, and the two trebuchet sides pick the matching row. Onager and fire onager use the attacker mangonel rows. Catapult and fire catapult use the defender mangonel rows.

## Where it lives

Managed script. No prefab stores it.

## Examples

**Naval map.** `NavalDLC\SceneObj\Main_map`, an `empty_object` with only this script. All ten names are the `_mapicon` prefabs listed above.

**Land map.** `SandBox\SceneObj\Main_map`, entity `__empty_object` at `700, 750, 0`. The same ten names.
