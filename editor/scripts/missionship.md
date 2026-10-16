# MissionShip

!!! info "v1.4.8"

The ship in a naval battle: hull, crew decks, sails, oars, and the navmesh soldiers walk on. Each packed ship prefab has one.

Find placed ones with `s:MissionShip`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the ship root, with the physics and the other ship scripts (`NavalPhysics`, the floaters, the cloth fixer). There is no loose prefab under `Prefabs`. The authored copies are the `*_ship_nested` entries in the packed naval prefab file.

Crew markers the ship collects by tag:

- `sp_troop_outer_deck` and `sp_troop_inner_deck` for formation spawn frames
- `sp_troop_captain`
- `sp_troop_crew_spawn`
- `rally_point`
- `banner_with_faction_color`
- `sail_mesh_entity` for sail meshes that take the faction colors

## What you set

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh prefab imported for this hull. Each ship stores its own. Examples below. |
| **ShouldUpdateSoundPos** | When on, the ship updates sound positions. Every packed ship opened stores this off (`0`). |

## What it does

The ship spawns troops on the deck tags, raises the banner and sail colors from the captain's faction, and drives sails, oars, the rudder, and attachments. **NavMeshPrefabName** is the walkable deck. Health, fire, and sinking come from the ship record, not from these two fields.

## Where it lives

`Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. Forty-three ship prefabs store the script, including `cog_ship_nested`, `dromon_ship_nested`, `drakkar_ship_nested`, `dhow_ship_nested`, `corbita_ship_nested`, and the storyline hulls.

## Examples

No scene file stores this script.

**Cog.** `cog_ship_nested`. **NavMeshPrefabName** `cog_ship_navmesh`. **ShouldUpdateSoundPos** off.

**Corbita.** `corbita_ship_nested`. **NavMeshPrefabName** `corbita_ship_navmesh`. **ShouldUpdateSoundPos** off.

**Roundship.** `crusas_roundship_nested_q5`. **NavMeshPrefabName** `roundship_navmesh`. **ShouldUpdateSoundPos** off.

**Catship.** `catship_ship_nested`. **NavMeshPrefabName** `catship_ship_navmesh`. **ShouldUpdateSoundPos** off. The medium catship stores `catship_medium_navmesh`.

**Burning storyline cog.** `burning_cog_ship_nested_storyline`. **NavMeshPrefabName** `burning_cog_ship_navmesh`. **ShouldUpdateSoundPos** off.

**Storyline fishing boat.** `burning_fishing_ship_nested_storyline`. **NavMeshPrefabName** `fishing_ship_navmesh_storyline`. **ShouldUpdateSoundPos** off.
