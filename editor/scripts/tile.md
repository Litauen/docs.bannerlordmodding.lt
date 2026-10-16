# Tile

!!! info "v1.4.8"

A board-game square. The mesh you assign is the tile the game shows and hides.

Find placed ones with `s:Tile`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on each square entity inside the board prefabs in `Modules\SandBox\Prefabs\sp_props_boardgames.xml`. The entity already carries the tile mesh.

## What you set

The board prefab stores one row.

| Panel field | What to do with it |
|---|---|
| **TileMesh** | Meta mesh of the square. The opened squares store `valid_tile`. |

## What it does

On init the entity drops its first meta mesh. The board game reads **TileMesh** back when it needs the square mesh, and it shows or hides the entity.

## Where it lives

Managed script. Prefab file: `Modules\SandBox\Prefabs\sp_props_boardgames.xml`.

## Examples

**Puluc square.** A tile entity in `sp_props_boardgames.xml` stores **TileMesh** `valid_tile`. The Puluc board reads that mesh from the script.

No `scene.xscene` stores this script.
