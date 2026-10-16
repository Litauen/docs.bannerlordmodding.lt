# VertexAnimator

!!! info "v1.4.8"

Loops a vertex morph on the entity's meshes, from a start key to an end key.

Find placed ones with `s:VertexAnimator`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the mesh entity that should morph. Flags, the main-menu scene mesh, and a dice board use it. The mesh needs morph keys between **BeginKey** and **EndKey**.

## What you set

`intro_flag` stores this order. **NavMeshPrefabName** is a panel field these prefabs do not write.

| Panel field | What to do with it |
|---|---|
| **Speed** | Keys per second. `intro_flag` stores `18`. The code default is `20`. |
| **BeginKey** | First morph key. The opened prefabs store `0`. |
| **EndKey** | Last morph key. `intro_flag` stores `250`. The dice board stores `600`. |
| **NavMeshPrefabName** | Left empty. The opened prefabs do not write this row. |

## What it does

On start the morph plays from **BeginKey** toward **EndKey** at **Speed**, then it loops. The entity's meshes take that morph time. A one-shot play stops on the end key. That one-shot flag is not a panel field.

## Where it lives

Managed script. `intro_flag` is in `Modules\Native\Prefabs\scene_prop_prefabs.xml`. The dice board is in `Modules\SandBox\Prefabs\sp_props_boardgames.xml`.

## Examples

**Intro flag.** `intro_flag`. Speed `18`, keys `0` to `250`. `intro_flag_2` is the next entity in the same file.

**Dice board.** `sp_props_boardgames.xml`. Speed `20`, keys `0` to `600`. A child is `DiceBoard_RollPosition`.

**Main menu.** `Native\SceneObj\main_menu_a`. The entity with mesh `intro_scene` stores speed `30`, keys `0` to `299`.

`Native\SceneObj\character_menu_a` and `SandBox\SceneObj\banner_editor_scene` store the script as well.
