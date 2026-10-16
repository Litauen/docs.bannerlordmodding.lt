# SmithingMachine

!!! info "v1.4.8"

The blacksmith bellows. A pilot stand pumps them, and a second stand is the anvil. The script adds no fields of its own.

Find placed ones in the Scene panel with `s:SmithingMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the machine entity, not on the stands. The entity needs a skeleton and two standing points. The pilot point is the one whose tag matches **PilotStandingPointTag**. The other point is the anvil. Vanilla machines use skeleton `merchant_smithing_machine_skeleton`.

## What you set

`SandBox\Prefabs\sp_props_merchandise.xml` stores this order on `sp_blacksmith_with_smithing_machine`. The nord shop copies store the same values.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Pilot stand tag. The machines store `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. The machines store `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. The machines store `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. The machines leave it empty. |

## What it does

Both stands start closed to the player. The description is Bellows, and the action is Use. While the pilot stands still, the machine plays `anim_merchant_smithing_machine_loop` and the pilot plays the matching use clip. When the pilot leaves, the machine returns to `anim_merchant_smithing_machine_idle_with_blend_in`. Using the anvil point switches the machine to the anvil state.

## Where it lives

Managed script. The prefab is `sp_blacksmith_with_smithing_machine` in `SandBox\Prefabs\sp_props_merchandise.xml`. `NavalDLC\Prefabs\architecture_nord.xml` embeds that machine twice. No scene file stores the script.

## Examples

**Merchandise prefab.** `sp_blacksmith_with_smithing_machine`. Tag `sp_blacksmith` on the nord copies. Pilot, ammo, and wait tags as in the table. Navmesh empty.

**Nord shop.** `architecture_nord.xml` places the same entity name twice, with the same four values and skeleton `merchant_smithing_machine_skeleton`.
