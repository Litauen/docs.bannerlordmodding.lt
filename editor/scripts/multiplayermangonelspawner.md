# MultiplayerMangonelSpawner

!!! info "v1.4.8"

Spawns the non-fire multiplayer mangonel. A name override can swap in another prefab, such as the grapeshot body.

Find placed ones in the Scene panel with `s:MultiplayerMangonelSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `mangonel_a_spawnerMP` or `mangonel_b_spawnerMP` from `Modules\Native\Prefabs\spawners.xml`. Empty **ToBeSpawnedOverrideName** spawns `mangonel_a` or `mangonel_b`. This spawner does not spawn the fire machine.

## What you set

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag added on deploy. Empty in vanilla. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty in vanilla. |
| **ammo_pos_a_enabled** … **ammo_pos_h_enabled** | One checkbox per ammo child. On by default. Off removes that child. |
| **ToBeSpawnedOverrideName** | Prefab to spawn. Empty uses the entity-name rule. |
| **ToBeSpawnedOverrideNameForFireVersion** | Fire prefab name. This spawner does not spawn it. |

**projectile_pile** is hidden. The `a` prefab stores it at about `-3.34, 4.38`. The `b` prefab stores it at about `-3.21, 2.76`.

## What it does

Spawns the non-fire mangonel, removes ammo children that are off, and copies the deploy tags.

## Where it lives

Managed script. Prefabs in `Modules\Native\Prefabs\spawners.xml`. Multiplayer siege scenes under `Modules\Native\SceneObj`.

## Examples

**Stock A and B.** `spawners.xml`. All eight ammo boxes on. No name override.

**Grapeshot body.** `Native\SceneObj\mp_siege_map_003`. `mangonel_b_spawnerMP` sets **ToBeSpawnedOverrideName** to `mangonel_b_grapeshot` and moves **projectile_pile**.
