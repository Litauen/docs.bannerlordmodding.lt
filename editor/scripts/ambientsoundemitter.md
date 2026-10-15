# AmbientSoundEmitter

!!! info "v1.4.8"

Plays a sound from an entity in the scene. The entity position is where the sound sits. **Event Path** picks the clip. Whether it keeps going, how far it carries, and the master mix belong to the sound, not to the checkboxes.

Find placed ones in the Scene panel with `s:AmbientSoundEmitter`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity.

- A room: scale the entity to the room, turn **Is Triggered** on, and set **Trigger Shape** to Box. The box is the entity’s scale.
- A sound you walk up to: leave the entity small and set a short **Overrided Max Distance**.

## What you set

| Panel field | What to do with it |
|---|---|
| **Event Path** | The field that matters. Paste an id such as `event:/mission/ambient/area/forest`. This is the value that stays after the scene is reopened. |
| **Play On Startup** | Flag on that sound. Vanilla room and bird prefabs leave it on. |
| **Is Static** | Flag on that sound. `khuzait_dungeon_a` sets it on for the dungeon bed. |
| **Override Sound** | Has to be on before the three numbers below are written onto the sound. |
| **Overrided Volume Multiplier** | Loudness for this placement. With **Override Sound** off, a reopen shows `0`. |
| **Overrided Max Distance** | How far this placement is heard. Bird prefabs store about `50`. |
| **Overrided Min Distance** | Full volume inside this radius. Bird prefabs store about `1.5`. |
| **Is Master Sound** | The main bed for a whole interior. The tavern prefab stores this on, with **Is Triggered** off. |
| **Is Master Supressor** | Checkbox on the script. `empire_town_f` stores it on for the city volumes. A fresh edit may not stick after reopen. |
| **Is Triggered** | On: the sound plays only while someone is inside the volume. Castle rooms store this on. |
| **Trigger Shape** | Box or Sphere. Stays grey until **Is Triggered** is on. Castle rooms use Box. |

Vanilla prefabs also store `Overrided Event Path (Winter)` and `Overrided Event Path (Siege)`. City, forest, and village use the winter path `event:/mission/ambient/area/winter`. City and village use the siege path `event:/mission/ambient/area/plains`.

The tavern bed, copied from `Modules\Native\Prefabs\sound_prefabs.xml`:

``` xml
<script name="AmbientSoundEmitter">
    <variables>
        <variable name="Event Path" value="event:/mission/ambient/area/interior/tavern"/>
        <variable name="Play On Startup" value="true"/>
        <variable name="Is Master Sound" value="true"/>
        <variable name="Is Master Supressor" value="false"/>
        <variable name="Is Triggered" value="false"/>
        <variable name="Trigger Shape" value="Sphere"/>
    </variables>
</script>
```

## What the player hears

- A forest, city, village, docks, or market bed: `event:/mission/ambient/area/forest` (also `city`, `desert`, `docks`, `lake`, `market`). These keep playing.
- A room that changes at the door: two scripts on the same scaled box. One is `event:/mission/ambient/area/interior/generic` (or `tavern`, `shop`, `cave`, `dungeon`, `keep`). The other is the reverb for that room: `event:/mission/ambient/reverb/wood_interior`, `stone_interior`, `stone_corridor`, or `gatehouse`.
- One bed for a whole tavern: `event:/mission/ambient/area/interior/tavern` with **Is Master Sound** on.
- Birds you only hear nearby: `event:/mission/ambient/detail/birds_city` (also `birds_forest`, `birds_coastal`, `birds_lake`, `birds_taiga`). Prefabs `detail_birds_*_sound`.

## Where the sound lives

- Ids: `Modules\Native\ModuleData\sound_event_data.gen.xml` (`path`, `length`, `max_distance`, `parameter_indices`).
- Audio: `Modules\Native\Sounds\PC\ambient.bank`. The master mix is `MasterBank.bank` in that same folder.
- Ready-made placements: `Modules\Native\Prefabs\sound_prefabs.xml`.

On an event row, `length="-1"` keeps going until something stops it. A positive `length` is one play, in seconds. `max_distance="100000"` is heard across the scene (forest, city, interior beds). A small number is positional. **Overrided Max Distance** on a bird placement is how vanilla keeps a wide event local.

`parameter_indices` is what that event accepts. Forest takes `Daytime,Rainfall`. City also takes `MissionCombatMode`.

See [Sound](/modding/sound/).

## Examples

Open these and filter with `s:AmbientSoundEmitter`. The scene stores what it changes. The event id sits on the prefab.

**Forest bed, kept near the camp.** `SandBox\SceneObj\bandit_forest`. One `forest_ambient_sound` at the camp. The prefab plays `event:/mission/ambient/area/forest` and is not triggered. The scene sets max distance `20` and min distance `1`.

**Forest zone and village bed in the same place.** `SandBox\SceneObj\vlandia_village_d`. Several `forest_ambient_sound` entities are scaled to about `132` and have **Is Triggered** on. One `village_ambient_sound` sits by the houses (`event:/mission/ambient/area/village`) with max distance `20`.

**City district.** `SandBox\SceneObj\empire_town_f`. Two `city_ambient_sound` entities. The prefab plays `event:/mission/ambient/area/city`, with **Is Triggered** on. One is scaled to about `187 × 143 × 52` and sets **Trigger Shape** to Box. Both set **Is Master Supressor**.

**One bed for a whole tavern.** `SandBox\SceneObj\empire_interior_tavern_a`. One `int_tavern_ambient_sound` in the room, with no extra values on the scene. **Is Master Sound** is on and **Is Triggered** is off. The same prefab is in `khuzait_tavern_a`, `aserai_tavern_interior`, and `sturgia_house_b_interior_tavern`.

**Reverb fitted to the room.** `SandBox\SceneObj\vlandia_tavern_interior_a`. `reverb_wood_interior` is scaled to about `16 × 10 × 10` and sets **Trigger Shape** to Box. The event is `event:/mission/ambient/reverb/wood_interior`, max distance `20`. `SandBox\SceneObj\empire_house_c_tavern_a` does the stone version: `reverb_stone_interior` scaled to about `18`.

**Dungeon bed, trigger turned off.** `SandBox\SceneObj\khuzait_dungeon_a`. The prefab is a triggered sphere on `event:/mission/ambient/area/interior/dungeon`. This scene turns **Is Triggered** off, turns **Is Master Sound** on, and sets max distance to `10000`. The same prefab, without that override, is in `empire_dungeon_a` and `sturgia_dungeon_a`.

**Birds you walk up to.** `SandBox\SceneObj\forest_hideout_004_sv`. `detail_birds_forest_sound` plays `event:/mission/ambient/detail/birds_forest`. The scene sets max distance `35` and min distance `3`.

**A river.** `SandBox\SceneObj\sea_bandit_d_sv`. `detail_river_01_sound` sits on the water and plays `event:/mission/ambient/detail/river_01`. The prefab’s reach is max `100`, min `2`. The scene sets the volume multiplier to `1`. The same scene also drops `detail_birds_forest_sound` in the trees.

**A room with two scripts on one box.** In `Native\Prefabs\castle_khuzait_l2.xml`, the child `sound_reverb` under `khuzait_castle_corner_e_l2` is scaled to the room. Both scripts have **Is Triggered** on and **Trigger Shape** Box. One plays `event:/mission/ambient/area/interior/generic`. The other plays `event:/mission/ambient/reverb/wood_interior`. Gatehouses in that file use `event:/mission/ambient/reverb/gatehouse`. Stone rooms use `stone_interior`.
