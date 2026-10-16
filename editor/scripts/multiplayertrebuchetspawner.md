# MultiplayerTrebuchetSpawner

!!! info "v1.4.8"

Spawns the non-fire multiplayer trebuchet. Ammo piles can be turned off one by one.

Find placed ones in the Scene panel with `s:MultiplayerTrebuchetSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `trebuchet_a_spawnerMP` from `Modules\Native\Prefabs\spawners.xml`. Empty **ToBeSpawnedOverrideName** spawns `trebuchet_a`. This spawner does not spawn the fire machine.

## What you set

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag added on deploy. Empty in vanilla. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty in vanilla. |
| **ammo_pos_a_enabled** … **ammo_pos_h_enabled** | One checkbox per ammo child. On by default. Off removes that child. |
| **ToBeSpawnedOverrideName** | Prefab to spawn. Empty uses the entity-name rule. |
| **ToBeSpawnedOverrideNameForFireVersion** | Fire prefab name. This spawner does not spawn it. |

**projectile_pile** is hidden. The prefab stores it at about `-5.48, 1.72`. Move that child.

## What it does

Spawns the non-fire trebuchet, removes ammo children that are off, and copies the deploy tags.

## Where it lives

Managed script. Prefab in `Modules\Native\Prefabs\spawners.xml`. `mp_siege_map_005` and its winter twin place it.

## Examples

**Stock trebuchet.** `spawners.xml`, `trebuchet_a_spawnerMP`. All eight ammo boxes on.

**Pile moved.** `Native\SceneObj\mp_siege_map_005`. Two `trebuchet_a_spawnerMP` entities. Each scene copy only rewrites **projectile_pile**.
