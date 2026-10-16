# ShadowingSecureZoneUsePoint

!!! info "v1.4.8"

A use point inside a secure-zone volume. The main agent can stand there and play an unarmed idle. The prompt is **Blend in**.

Find placed ones in the Scene panel with `s:ShadowingSecureZoneUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

The vanilla placement is the prefab `shadowing_secure_zone` in `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml`. The parent is tagged `shadowing_mission_secure_zone` and has a `VolumeBox`. The use point is the child `secure_zone_use_point`: editor-only, blue stand arrow, raycast-only physics.

No scene instances that prefab.

## What you set

The script adds no fields of its own. The prefab stores the one inherited row.

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh prefab to import. The secure-zone point leaves it empty. |

``` xml
<script name="ShadowingSecureZoneUsePoint">
    <variables>
        <variable name="NavMeshPrefabName" value=""/>
    </variables>
</script>
```

## What it does

The use text is **Blend in**. Only the main agent can use it. On use they play `act_idle_unarmed_1`. When they stop, that action is cleared.

## Where it lives

Managed script on a usable mission object. Prefab: `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml` (`shadowing_secure_zone`).

## Examples

**Secure-zone stand.** Prefab `shadowing_secure_zone`. Parent tag `shadowing_mission_secure_zone`, script `VolumeBox`, **NavMeshPrefabName** empty. Child `secure_zone_use_point` has this script, the same empty navmesh name, mesh `animation_stand_arrow_blue`, and physics `bo_barrier_box` set to collide only with raycasts.

No `scene.xscene` under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC instances the prefab or stores the script.
