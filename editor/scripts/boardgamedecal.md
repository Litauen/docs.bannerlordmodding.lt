# BoardGameDecal

!!! info "v1.4.8"

A decal on a board-game tile. It starts invisible. The game fades it in to mark a legal move. The script has no panel fields.

Find placed ones in the Scene panel with `s:BoardGameDecal`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on a small decal entity under the board, one per tile. Tag the entity `decal_` plus the tile coordinates the game looks up (`decal_4_3`, `decal_0`, `decal_homebase_player`).

## What you set

Nothing. The script declares no visible field, and the board prefabs store no variables.

## What it does

On mission start the decal alpha is `0`. When a tile is a legal move, the game fades that decal in over about a fifth of a second. When the tile is no longer legal, it fades back out.

## Where it lives

Managed script. Boards are in `SandBox\Prefabs\sp_props_boardgames.xml`. No scene file stores the script. The games that read the tags are Seega, Tablut, Konane, Bagh Chal, Puluc, and Mu Torere.

## Examples

**Seega tile.** `SandBox\Prefabs\sp_props_boardgames.xml`, entity `BoardGameSeega_FullSetup` (tag `boardgame_aserai`). Child `decal_4_3` is tagged `decal_4_3`, scaled about `0.029`, material `blood_decal_b`, and has the script with no variables. The same board has a `decal_` child for each other cell.
