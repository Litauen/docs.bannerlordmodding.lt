# PlayMusicPoint

!!! info "v1.4.8"

One musician's standing point. The loop animation is the performance. Players cannot use it. A **MusicianGroup** parent swaps the animation for the current instrument.

Find placed ones in the Scene panel with `s:PlayMusicPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Child of the musician stand, under a **MusicianGroup**. The parent entity's tags are what the group matches to an instrument (`musician` on `sp_standing_musician`). The point itself is editor-only.

## What you set

`sp_npc_musicians_a` stores the rows in this order. The clip is the field that matters until a track replaces it.

| Panel field | What to do with it |
|---|---|
| **ArriveAction** | Arrival clip. Empty on the band. |
| **LoopStartAction** | Loop while in use. The band uses `act_play_dombra_stand`. The dancer point uses `act_dance_norse`. |
| **PairLoopStartAction** | Pair loop. Empty. |
| **LeaveAction** | Leave clip. Empty. |
| **GroupId** | Group id. `-1`. |
| **RightHandItem** | Right-hand prop. Empty. The group attaches the instrument itself. |
| **RightHandItemBone** | `ItemR`. |
| **LeftHandItem** | Empty. |
| **LeftHandItemBone** | `ItemL`. |
| **MinUserToStartInteraction** | `1`. |
| **ActivatePairs** | Off. |
| **MinWaitinSeconds** | Band stores `-1`. Code default is `30`. The dancer point uses `30`. |
| **MaxWaitInSeconds** | Band stores `-1`. Code default is `120`. The dancer point uses `120`. |
| **ForwardDistanceToPivotPoint** | `0`. |
| **SideDistanceToPivotPoint** | `0`. |
| **KeepOldVisibility** | Off. |
| **AutoSheathWeapons** | Band stores off. Code default is on. The dancer point stores on. |
| **AutoEquipWeaponsOnUseStopped** | Off. |
| **AutoWieldWeapons** | Off. |
| **TranslateUser** | On. |
| **HasRecentlyBeenRechecked** | Prefab writes false. Leave it. |
| **_useOwnPositionInsteadOfWorldPosition** | Off. |
| **_customPlayerInteractionDistance** | `0`. |
| **NavMeshPrefabName** | Empty. |

## What it does

The point is closed to the player. While a track is playing and someone is on the point, they loop **LoopStartAction**. The group replaces that clip with the instrument's sit or stand action and attaches the instrument meshes. When the track ends, the loop stops and the instrument comes off.

## Where it lives

Managed script. Inside the musician prefabs in `Modules\SandBox\Prefabs\sp_editor_paired_spawnpoints.xml` and the dancer prefab in `sp_editor_spawnpoints.xml`.

## Examples

**Dombra stand.** `sp_npc_musicians_a`, child `play_music_point`. Loop `act_play_dombra_stand`. Parent tagged `musician`.

**Dancer.** `sp_npc_dancing_with_audience`. Loop `act_dance_norse`, waits `30` to `120`.

**Town, prefab unchanged.** `SandBoxCore\SceneObj\empire_town_s` places `sp_npc_musicians_a` through `sp_npc_musicians_c` and does not rewrite the point.
