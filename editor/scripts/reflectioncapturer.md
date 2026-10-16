# ReflectionCapturer

!!! info "v1.4.8"

A reflection probe. **IsGlobal** is one probe for the whole scene. Off, it is a local probe you scale over an area.

Find placed ones in the Scene panel with `s:ReflectionCapturer`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity, tag it `envmap_probe`, and hide it outside the editor. The ready-made prefabs are `envmap_prop` (global) and `local_envmap_prop` (local) in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`.

Scale a local probe over the area it covers. A global probe can stay at scale `1`.

## What you set

Native panel. Both prefabs store the same order.

| Panel field | What to do with it |
|---|---|
| **IsGlobal** | On: one probe for the scene. Off: local. `envmap_prop` stores on. `local_envmap_prop` stores off. |
| **IsParallaxCorrected** | On: the reflection is corrected to a box. Both prefabs store off. Interiors and large local probes turn it on. |
| **AmbientMultiplier** | Ambient scale. Every probe opened below stores `1`. |
| **AttenuationCoef** | Falloff. Every probe opened below stores `20`. |
| **BoxOffset** | Shifts the parallax box. Prefabs store zero. A siege probe stores an X of `-21.5`. |

`envmap_prop`:

``` xml
<script name="ReflectionCapturer">
    <variables>
        <variable name="IsGlobal" value="true"/>
        <variable name="IsParallaxCorrected" value="false"/>
        <variable name="AmbientMultiplier" value="1.000"/>
        <variable name="AttenuationCoef" value="20.000"/>
        <variable name="BoxOffset" value="0.000, 0.000, 0.000, 0.000"/>
    </variables>
</script>
```

## What it does

The entity is the probe. Global probes sit once in a scene and leave parallax off. A room or a large outdoor box turns parallax on. The local prefab is the one scenes scale.

## Where it lives

Native script. Prefabs: `Modules\Native\Prefabs\editor_helpers_&_tests.xml`.

## Examples

**Editor scene, one global probe.** `Native\SceneObj\flora_editor_scene`. Entity `envmap_probe` at `(20, 0, 5)`. **IsGlobal** on, parallax off.

**Training field.** `StoryMode\SceneObj\training_field`. Entity `envmap_probe`. Same five values as the global prefab.

**Castle keep, global but parallax-corrected.** `SandBox\SceneObj\empire_castle_keep_a_l3_interior`. Entity `envmap_probe`. **IsGlobal** on and **IsParallaxCorrected** on. The entity is on levels 1, 2, 3, and civilian.

**Siege map, both kinds.** `Native\SceneObj\mp_siege_map_005`. One `envmap_probe` is global with parallax off. One `local_envmap_prop` is scaled to about `105 × 140 × 43`, turns parallax on, and sets **BoxOffset** X to `-21.5`.
