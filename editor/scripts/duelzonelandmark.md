# DuelZoneLandmark

!!! info "v1.4.8"

A focusable flag that sets the player's preferred duel troop type. The type is **ZoneTroopType**.

Find placed ones in the Scene panel with `s:DuelZoneLandmark`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the flag entity. Tag that entity `duel_zone_landmark`. The duel maps also tag the pole with the troop it stands for (`flag_infantry`, `flag_archery`, or `flag_cavalry`).

## What you set

| Panel field | What to do with it |
|---|---|
| **ZoneTroopType** | `Infantry`, `Ranged`, or `Cavalry`. `Invalid` is the unset value. |

## What it does

When the player focuses the flag and confirms, the client asks to change their preferred troop type to **ZoneTroopType**. That request is refused while they are already in a duel. The focus description is empty.

## Where it lives

Managed script. The three duel layouts are under `Native\SceneObj`.

## Examples

**Three flags on the day map.** `Native\SceneObj\mp_duel_mode_map_004`.

- Infantry: tag `area_flag_6`, also `flag_infantry` and `flag_f`. **ZoneTroopType** `Infantry`.
- Ranged: tag `area_flag_14`, also `flag_archery` and `flag_b`. **ZoneTroopType** `Ranged`.
- Cavalry: tag `area_flag_9`, also `flag_cavalry` and `flag_a`. **ZoneTroopType** `Cavalry`.

Each is a `flag_pole_big` entity tagged `duel_zone_landmark`.

**Night and winter copies.** `mp_duel_mode_map_004_night` and `mp_duel_mode_map_004_w` store the same three values: `Infantry`, `Ranged`, and `Cavalry`.
