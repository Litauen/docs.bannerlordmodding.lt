# reverb_instance

!!! info "v1.4.8"

Plays a reverb snapshot from an entity. **Snapshot Name** is the event. **Shape** is the zone.

Find placed ones in the Scene panel with `s:reverb_instance`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity you can scale over the room. The ready-made prefab is `sound_reverb` in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. Hide it outside the editor if you only need the zone.

## What you set

Native panel. The prefab stores this order.

| Panel field | What to do with it |
|---|---|
| **Snapshot Name** | The event id, such as `event:/mission/ambient/reverb/wood_interior`. The prefab stores an empty string. |
| **Shape** | Sphere, or left empty. The prefab stores `Sphere`. |

``` xml
<script name="reverb_instance">
    <variables>
        <variable name="Snapshot Name" value=""/>
        <variable name="Shape" value="Sphere"/>
    </variables>
</script>
```

## What the player hears

`event:/mission/ambient/reverb/wood_interior` keeps playing (`length="-1"`) and its event `max_distance` is `100000`. It takes no sound parameters. The entity scale is the zone. A village tent scales the child to about `2.2 × 1 × 1`.

Ids: `Modules\Native\ModuleData\sound_event_data.gen.xml`.

## Where it lives

Native script. Prefab: `Modules\Native\Prefabs\editor_helpers_&_tests.xml`, entity `sound_reverb`.

## Examples

**Empty zone, sphere shape.** Prefab `sound_reverb`. **Snapshot Name** is empty. **Shape** is `Sphere`.

**Wood interior inside a tent.** `SandBox\SceneObj\vlandia_village_b`. Entity `tent_vlandia_c` has a child `sound_reverb` scaled to about `2.2 × 1 × 1`. **Snapshot Name** is `event:/mission/ambient/reverb/wood_interior`. **Shape** is `Sphere`.

**Tavern zones with the name left blank.** `SandBox\SceneObj\empire_interior_tavern_a`. Entities `sound_reverb_zone_01` and `sound_reverb_zone_02` use the sound icon and are editor-only. Both store an empty **Snapshot Name** and an empty **Shape**.
