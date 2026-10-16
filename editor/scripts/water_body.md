# water_body

!!! info "v1.4.8"

A water surface on an editor plane. **Water Prefab** picks the water type.

Find placed ones in the Scene panel with `s:water_body`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop the prefab `water_plane` from `Modules\Native\Prefabs\editor_contents.xml`, then scale it over the lake or sea. The mesh is `editor_plane_high`. Physics shape on the prefab is `bo_editor_plane`.

Campaign seas are huge scaled planes. A battle puddle is a smaller plane with physics material `water`.

## What you set

Native panel.

| Panel field | What to do with it |
|---|---|
| **Water Prefab** | Water type. The empty prefab stores a blank string. Campaign seas store `Main Map Sea`. Muddy ground stores `Muddy Puddle`. |

``` xml
<script name="water_body">
    <variables>
        <variable name="Water Prefab" value=""/>
    </variables>
</script>
```

## What it does

The plane is the water. **Water Prefab** selects which water it is. A campaign sea stores `Main Map Sea`. A puddle stores `Muddy Puddle`.

## Where it lives

Native script. Prefab: `Modules\Native\Prefabs\editor_contents.xml`, entity `water_plane`.

## Examples

**Empty plane.** Prefab `water_plane`. **Water Prefab** is empty. Mesh `editor_plane_high`.

**Campaign sea.** `SandBox\SceneObj\Main_map`. Prefab `water_plane` scaled to about `71 × 69`, mesh material `worldmap_lake_clear`. **Water Prefab** is `Main Map Sea`. Another plane in that scene stores `Muddy Puddle`.

**Muddy skirmish water.** `Native\SceneObj\mp_skirmish_map_014`. Prefab `water_plane`, physics material `water`, mesh material `wat_puddle_muddy`. **Water Prefab** is `Muddy Puddle`. Five planes in that scene use it.
