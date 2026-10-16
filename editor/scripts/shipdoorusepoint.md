# ShipDoorUsePoint

!!! info "v1.4.8"

A use point that opens a ship door for the player. An empty action id uses the text **Open**, and an empty description id uses **Door**.

Find placed ones in the Scene panel with `s:ShipDoorUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the use point in front of the door. A child tagged `ship_door_highlight` is the marker. The point stays disabled until mission code enables it, and only the player can use it, from within `2` meters.

The loose prefab is `ship_door_use_point` in `NavalDLC\Prefabs\NavalDLC_storyline.xml`.

## What you set

The prefab stores these rows, in this order.

| Panel field | What to do with it |
|---|---|
| **ActionStringId** | Game text id for the use line. Empty uses `str_open_ship_door`. The prefab stores empty. |
| **DescriptionStringId** | Game text id for the description. Empty uses `str_ui_door`. The prefab stores empty. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab leaves it empty. |

## What it does

While the point is enabled, the player gets the action text and a door-open sound (`event:/mission/movement/foley/door_open`). The highlight child is kept hidden. The point does not stay occupied.

## Where it lives

Managed script on a usable point. Prefab `ship_door_use_point` in `NavalDLC\Prefabs\NavalDLC_storyline.xml`. A packed copy is `storyline_mesh_holder` in `nested_prefabs_packed.xml`.

## Examples

**Storyline prefab.** `NavalDLC\Prefabs\NavalDLC_storyline.xml`, entity `ship_door_use_point`. Both string ids empty. Child `highlight_pointer_glow_ground` is tagged `ship_door_highlight`. The door mesh is `nord_grand_bazaar_part_1_door`.

**Placed on the quest scene.** `NavalDLC\SceneObj\naval_storyline_act_3_quest_5`. Entity `ship_door_use_point`, tag `phase_1_interior_to_enemy_ship_3_door_tag`. The three rows stay empty. Position about `2116.5, 700.9, 37.8`.

**Packed copy.** Prefab `storyline_mesh_holder`, entity `ship_door_use_point`. The same three empty rows.
