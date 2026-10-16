# MusicianGroup

!!! info "v1.4.8"

A band. Child points are the musicians. When at least one is in use, the group plays a settlement track from this entity, then waits 8 seconds and plays the next.

Find placed ones in the Scene panel with `s:MusicianGroup`. See [Filter](/editor/editor/#filter).

## Where you put it

Use a musicians prefab and leave the child **PlayMusicPoint** entities under it. The parent of each music point needs the instrument tag the track asks for (`musician` on the standing-musician prefab). Vanilla groups are `sp_npc_musicians_a` through `sp_npc_musicians_d` in `Modules\SandBox\Prefabs\sp_editor_paired_spawnpoints.xml`. `sp_npc_dancing_with_audience` in `sp_editor_spawnpoints.xml` is the same script with a dancer point.

## What you set

This script adds no fields. The prefab stores the usable-machine rows:

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Tag of the pilot point. Prefab `Pilot`. |
| **AmmoPickUpTag** | Ammo pickup tag. Prefab `ammopickup`. Unused by the band. |
| **WaitStandingPointTag** | Wait-point tag. Prefab `Wait`. |
| **NavMeshPrefabName** | Left empty. |

The track list is not a panel field. A town or village mission fills it when the mission starts.

## What the player hears

A settlement track from the mission location. The center also pulls `lordshall` and `tavern` tracks. A port also pulls `tavern`. The clip starts at this entity. Musicians already on a point loop with the track. When the last musician leaves, the clip stops. The next track starts after 8 seconds once someone is on a point again. Tempo is relative to 120. Spare points with no matching instrument tag get a cheerful, active, or calm idle from that tempo.

## Where it lives

Managed script. Prefabs in `Modules\SandBox\Prefabs\sp_editor_paired_spawnpoints.xml` and `sp_editor_spawnpoints.xml`. Town scenes place the prefabs without extra variables.

## Examples

**Four band layouts.** `sp_npc_musicians_a` through `sp_npc_musicians_d`. The standing point under `a` is tagged `musician` and plays `act_play_dombra_stand`.

**Dancer group.** `sp_npc_dancing_with_audience`. The music point uses `act_dance_norse`.

**Town placement, no overrides.** `SandBoxCore\SceneObj\empire_town_s`. `sp_npc_musicians_a`, `sp_npc_musicians_b`, and `sp_npc_musicians_c` on levels 1–3 and civilian. The scene does not change the script.
