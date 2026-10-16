# sound_emitter

!!! info "v1.4.8"

Plays one sound from an entity. **Event Path** picks the clip. Reach, length, and parameters belong to that event. The override numbers change this placement.

Find placed ones in the Scene panel with `s:sound_emitter`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity at the spot the sound should sit. The ready-made prefab is `_sound_emitter` in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. It uses mesh `icon_sound` and is visible only while editing.

## What you set

Native panel. The prefab stores this order. A tavern placement also stores **Is Static** after **Play On Startup**.

| Panel field | What to do with it |
|---|---|
| **Event Path** | The id that matters, such as `event:/mission/ambient/arena/ambience`. The prefab stores empty. |
| **Overrided Parameters** | On: this placement overrides the event's parameters. The prefab stores off. |
| **Play On Startup** | Start when the scene starts. The prefab stores on. |
| **Is Static** | Stored by the tavern placement, off. The helper prefab does not write it. |
| **Overrided Max Distance** | How far this placement is heard. The prefab stores `0`. The arena stores `10`. |
| **Overrided Min Distance** | Full volume inside this radius. The prefab stores `0`. The arena stores `5`. |
| **Overrided Volume Multiplier** | Loudness for this placement. The helper prefab stores `0.44`. The vineyard icon prefab stores `0`. |
| **Overrided Pitch** | Pitch for this placement. The prefab stores `0`. |

``` xml
<script name="sound_emitter">
    <variables>
        <variable name="Event Path" value=""/>
        <variable name="Overrided Parameters" value="false"/>
        <variable name="Play On Startup" value="true"/>
        <variable name="Overrided Max Distance" value="0.000000"/>
        <variable name="Overrided Min Distance" value="0.000000"/>
        <variable name="Overrided Volume Multiplier" value="0.440000"/>
        <variable name="Overrided Pitch" value="0.000000"/>
    </variables>
</script>
```

## What the player hears

- Arena bed: `event:/mission/ambient/arena/ambience`. `length="-1"` keeps it going. The event's own `max_distance` is `10`. It takes `ArenaIntensity`.
- An empty emitter, as in the tavern kitchen, has a blank **Event Path** and plays nothing until you set one.

Ids: `Modules\Native\ModuleData\sound_event_data.gen.xml`.

## Where it lives

Native script. Prefab `_sound_emitter` is in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. The vineyard icon prefabs in `Modules\Native\Prefabs\map_icon_parts.xml` also carry it.

## Examples

**Arena bed.** `SandBox\SceneObj\arena_empire_a`. Prefab `_sound_emitter` at about `(339, 437, 13)`. **Event Path** `event:/mission/ambient/arena/ambience`, volume `1`, max distance `10`, min distance `5`.

**Kitchen emitter with no clip.** `SandBox\SceneObj\empire_interior_tavern_a`. Entity `_sound_emitter`, editor-only, mesh `icon_sound`. **Event Path** is empty, **Play On Startup** on, **Is Static** off, both distances `0`.

**Vineyard icon.** `Native\Prefabs\map_icon_parts.xml`, entity `map_icons_production_vineyard_a`. **Event Path** empty, **Play On Startup** on, volume multiplier `0`.
