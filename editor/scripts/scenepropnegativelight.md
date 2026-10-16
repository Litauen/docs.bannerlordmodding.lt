# ScenePropNegativeLight

!!! info "v1.4.8"

A darkening volume on the mesh `negative_light_mesh`. Flatness shapes the volume. **Alpha** is how strong it is. **Is_Dark_Light** picks the dark-light shader path.

Find placed ones in the Scene panel with `s:ScenePropNegativeLight`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity with `negative_light_mesh`, then scale the entity over the area. Prefabs are in `Modules\Native\Prefabs\lighting.xml`.

The prefab named `_positive_light` uses this script, not [ScenePropPositiveLight](/editor/scripts/sceneproppositivelight/).

## What you set

| Panel field | What to do with it |
|---|---|
| **Flatness_X** | Flatness along X. Default `0`. |
| **Flatness_Y** | Flatness along Y. Default `0`. |
| **Flatness_Z** | Flatness along Z. Default `0`. |
| **Alpha** | Strength. Default `1`. |
| **Is_Dark_Light** | On: dark light. Off: the other path, used by `_dark_ambient_light`. Default on. |

`_negative_light`:

``` xml
<script name="ScenePropNegativeLight">
    <variables>
        <variable name="Flatness_X" value="0.000"/>
        <variable name="Flatness_Y" value="0.000"/>
        <variable name="Flatness_Z" value="0.000"/>
        <variable name="Alpha" value="1.000"/>
        <variable name="Is_Dark_Light" value="true"/>
    </variables>
</script>
```

## What it does

The four numbers are written to the mesh vector. **Is_Dark_Light** on writes a second vector of `(1, 0, 0, 0)`. Off writes zeros. Init and the editor tick both apply it.

## Where it lives

Managed script. Prefabs: `Modules\Native\Prefabs\lighting.xml`.

## Examples

**Default dark volume.** Prefab `_negative_light`. Mesh `negative_light_mesh`. All flatness `0`, **Alpha** `1`, **Is_Dark_Light** on. Prefab `_dark_light` stores the same values. Prefab `_positive_light` stores this script with those same values.

**Ambient darkening, not the dark-light path.** Prefab `_dark_ambient_light`. **Is_Dark_Light** off. The mesh material is `deferred_dark_ambient_lighting_material`.

**Hatch room, stronger and flatter.** `NavalDLC\SceneObj\cutscene_saving_sister`. Prefab `_dark_ambient_light`, scaled to `4`. The scene sets flatness to `0.37`, `0.27`, `0.3` and **Alpha** to `2`.

**Storm icon.** `NavalDLC\Prefabs\naval_map_icons.xml`. Prefab `psys_mapicon_typhoon`, child `_dark_light`, scaled to `60 × 60 × 30`. Flatness `0.1` on each axis, **Alpha** `1`, **Is_Dark_Light** on.

**Rain clouds, a bit rounder.** Same file, prefab `psys_mapicon_lightclouds`, child `_dark_light`, scaled to `60 × 60 × 30`. Flatness `0.2` on each axis.

**Sandstorm, slightly under full strength.** Same file, prefab `psys_mapicon_sandstorm`, child `_dark_light`, scaled to `40 × 40 × 30`. **Alpha** `0.97`, flatness `0.1`.
