# ShipFloatsamManager

!!! info "v1.4.8"

Spawns floating debris, scrape particles, and damage decals when the ship hits something. The class has no panel fields. The script name is spelled **Floatsam**.

Find placed ones in the Scene panel with `s:ShipFloatsamManager`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ship root. It looks for a child tagged `body_mesh`, and it reads [ShipColorAssigner](/editor/scripts/shipcolorassigner/) on the same entity for the debris color and the decal color.

## What you set

The class declares no editor fields. Placements store an empty script tag.

## What it does

A light hit can drop a barrel, a door, or `floatable_debris_barrel_a`. A ram or a scrape drops planks (`floatable_debris_plank_b` and the `e` through `k` set, skipping `c`, `d`, and `i`). A lost rudder, oar, mast, or shield uses `floatable_debris_rudder`, `floatable_debris_oar_a`, `floatable_debris_mast`, and `floatable_debris_` plus the shield name. Collision decals are `decal_ship_damaged_a` through `c`. Scrape decals are `decal_ship_damage_02` through `04`. Decals stop after `30`. Debris needs an impulse of `150000` before it breaks off. Scrape, mid, and heavy hit particles are `psys_game_ship_scrape_emit_on_move`, `psys_game_ship_collision`, `psys_naval_ship_hit_mid`, and `psys_naval_ship_hit_large`.

## Where it lives

Managed script. Packed ship prefabs in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Cog and corbita.** Packed prefabs `cog_ship_nested` and `corbita_ship_nested`. Empty script tag on the ship root.

**Burning storyline hulls.** Packed prefabs `burning_cog_ship_nested_storyline` and `burning_fishing_ship_nested_storyline`. The same empty tag. Those hulls also carry [ShipClothFixer](/editor/scripts/shipclothfixer/).
