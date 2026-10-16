# path_navigator

!!! info "v1.4.8"

Stores a path id and a **Speed** on the entity that should travel that path.

Find placed ones with `s:path_navigator`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the prop that should travel. Ships and birds use it. **Path Name** must match a path in the scene. No prefab stores the script; scenes place it on the entity.

## What you set

Native panel. The ships store this order.

| Panel field | What to do with it |
|---|---|
| **Path Name** | Path id. |
| **Speed** | How fast it travels. Ships use `3` and `4`. Birds use `1.4`. |
| **path_cache_point_index** | Stored point index. Every opened placement stores `0`. |

## What it does

Ships and birds store **Path Name** and **Speed**. The cache index stays `0` on every one opened below. The bird also has an `animation_instance` for the flying clip.

## Where it lives

Native script. Placements are in Native multiplayer scenes.

## Examples

**Slow ship.** `Native\SceneObj\mp_tdm_map_001`. Entity `ship_a`. **Path Name** `path4`, **Speed** `3`, cache index `0`.

**Faster ship.** Same scene, a second `ship_a`. **Path Name** `path_ship`, **Speed** `4`, cache index `0`.

**Birds.** `Native\SceneObj\mp_sergeant_map_005`. Entity `Bird`. One uses **Path Name** `bird`, the next uses `forest`. Both store **Speed** `1.4` and cache index `0`.
