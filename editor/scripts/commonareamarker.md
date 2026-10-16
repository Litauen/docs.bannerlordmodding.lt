# CommonAreaMarker

!!! info "v1.4.8"

Marks an alley. **AreaIndex** picks which alley in the settlement this marker is. **AreaRadius** is how far it claims usable machines.

Find placed ones in the Scene panel with `s:CommonAreaMarker`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop prefab `common_area_1`, `common_area_2`, or `common_area_3` where the alley is. Those prefabs are tagged `alley_marker`. The name-marker pass only lists markers that have that tag.

## What you set

| Panel field | What to do with it |
|---|---|
| **AreaRadius** | Meters around the entity. Prefab default `12`. |
| **AreaIndex** | Which alley. `1` is the settlement's first alley, `2` the second. `0` matches none. |
| **Type** | Stored string. Prefabs leave it empty. The marker methods that pick the alley do not read it. |
| **CheckToggle** | Area-marker helper. Opening the entity in the editor clears it. Prefabs do not store it. |

## What it does

**AreaIndex** selects `settlement.Alleys[AreaIndex - 1]`. The map marker name is that alley's name, and only if the entity is tagged `alley_marker` and the alley has an owner.

Inside **AreaRadius** the marker keeps usable machines tagged `npc_common`, `npc_common_limited`, `sp_guard`, `sp_guard_unarmed`, or `sp_notable`. Passages are dropped. Entities tagged `sp_common_hidden` inside the radius are extra spawn frames. A hideout uses those machines as places to put defenders.

## Where it lives

Managed script on the area-marker base. Prefabs are in `SandBox\Prefabs\sp_editor_spawnpoints.xml`.

## Examples

**First alley, default radius.** Prefab `common_area_1`. Tag `alley_marker`, **AreaRadius** `12`, **AreaIndex** `1`, **Type** empty. `common_area_2` is index `2`. `common_area_3` is index `3`.

**Village alley pulled wider, type filled in.** `SandBoxCore\SceneObj\vlandia_village_n`. One `common_area_1` sets **Type** to `Thicket` and **AreaRadius** to `17.960`. **AreaIndex** stays the prefab's `1`.

**Forest camp indexes.** `SandBox\SceneObj\bandit_forest`. `common_area_3` stores **AreaIndex** `0`. `common_area_2` stores **AreaIndex** `2`.
