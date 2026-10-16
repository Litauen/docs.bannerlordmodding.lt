# FightAreaMarker

!!! info "v1.4.8"

One room of a lord's-hall fight. **AreaIndex** is the room. **SubAreaIndex** splits that room. Defenders spawn inside **AreaRadius**.

Find placed ones in the Scene panel with `s:FightAreaMarker`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop prefab `fight_area_1`, `fight_area_2`, or `fight_area_3` in the keep. The scene can override **AreaRadius** on that instance. The hall fight collects every marker; it does not look for a special tag.

## What you set

| Panel field | What to do with it |
|---|---|
| **AreaRadius** | Meters. Prefab default `12`. Keep interiors that were opened set it to `1`. |
| **AreaIndex** | Room number. `fight_area_1` is `1`, `fight_area_2` is `2`, `fight_area_3` is `3`. |
| **SubAreaIndex** | Split inside that room. Prefab default `1`. |
| **CheckToggle** | Area-marker helper. Opening the entity in the editor clears it. Prefabs do not store it. |

The fight-area prefabs also store an empty **Type** row. This class has no **Type** field. The hall fight does not read it.

## What it does

The lord's-hall fight groups markers by **AreaIndex**, then by **SubAreaIndex**, and spawns the defender side into those groups. `GetAgentsInRange` later returns agents of a team or a battle side whose position is inside **AreaRadius**.

## Where it lives

Managed script on the area-marker base. Prefabs are in `SandBox\Prefabs\sp_editor_spawnpoints.xml`.

## Examples

**Three room prefabs.** `fight_area_1` stores **AreaRadius** `12`, **AreaIndex** `1`, **SubAreaIndex** `1`. `fight_area_2` is index `2`. `fight_area_3` is index `3`. **Type** is empty on each.

**Keep room, radius pulled in.** `SandBox\SceneObj\empire_castle_keep_a_l1_interior`. A `fight_area_1` near `sp_defender_archer_lords_hall` sets **AreaRadius** to `1`. Index and sub-index stay on the prefab. That scene contains 44 copies of the script.
