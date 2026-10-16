# MultiplayerFireMangonelSpawner

!!! info "v1.4.8"

Spawns the fire multiplayer mangonel, and can turn individual ammo piles off.

Find placed ones in the Scene panel with `s:MultiplayerFireMangonelSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `mangonel_a_spawnerFireMP` or `mangonel_b_spawnerFireMP` from `Modules\Native\Prefabs\spawners.xml`. Empty override names spawn `mangonel_a_fire` or `mangonel_b_fire`.

## What you set

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag added on deploy. Empty in vanilla. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty in vanilla. |
| **ammo_pos_a_enabled** … **ammo_pos_h_enabled** | One checkbox per ammo child. On by default. Off removes the child of that name (`ammo_pos_c` and so on). |
| **ToBeSpawnedOverrideName** | Base prefab name. Empty uses the entity name minus the last `_piece`. |
| **ToBeSpawnedOverrideNameForFireVersion** | Full fire prefab name. Empty adds `_fire` to the base name. |

**projectile_pile** is saved on the prefab and on scenes, and the panel hides that row. Move the `projectile_pile` child. The `a` prefab stores the pile at about `-3.34, 4.38`. The `b` prefab stores it at about `-3.21, 2.76`.

## What it does

Spawns only the fire mangonel. Ammo children whose checkbox is off are removed. Deploy tags are copied onto the machine.

## Where it lives

Managed script. Prefabs in `Modules\Native\Prefabs\spawners.xml`. Multiplayer siege scenes under `Modules\Native\SceneObj`.

## Examples

**Fire mangonel A and B.** `spawners.xml`. All eight ammo boxes on.

**Three piles removed.** `Native\SceneObj\mp_siege_map_005`. `mangonel_a_spawnerFireMP` moves **projectile_pile** and sets **ammo_pos_c_enabled**, **ammo_pos_d_enabled**, and **ammo_pos_e_enabled** to false.
