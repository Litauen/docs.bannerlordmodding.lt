# MultiplayerFireTrebuchetSpawner

!!! info "v1.4.8"

Spawns the fire multiplayer trebuchet. Same ammo checkboxes as the normal trebuchet spawner. This one spawns only the fire prefab.

Find placed ones in the Scene panel with `s:MultiplayerFireTrebuchetSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `trebuchet_a_spawnerFireMP` from `Modules\Native\Prefabs\spawners.xml`. Empty override names spawn `trebuchet_a_fire`.

## What you set

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag added on deploy. Empty in vanilla. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty in vanilla. |
| **ammo_pos_a_enabled** … **ammo_pos_h_enabled** | One checkbox per ammo child. On by default. Off removes that child. |
| **ToBeSpawnedOverrideName** | Base prefab name. Empty uses the entity name minus the last `_piece`. |
| **ToBeSpawnedOverrideNameForFireVersion** | Full fire prefab name. Empty adds `_fire`. |

**projectile_pile** is hidden on the panel. The prefab stores it at about `-5.48, 1.72`. Move that child to place the pile.

## What it does

Spawns only the fire trebuchet, drops ammo children that are switched off, and copies the deploy tags.

## Where it lives

Managed script. Prefab in `Modules\Native\Prefabs\spawners.xml`.

## Examples

**Fire trebuchet prefab.** `spawners.xml`, `trebuchet_a_spawnerFireMP`. All eight ammo boxes on. Pile at about `-5.48, 1.72`.

**Pile moved, ammo left on.** `Native\SceneObj\mp_siege_map_006_vlandia`. One `trebuchet_a_spawnerFireMP`. The scene only rewrites **projectile_pile**.
