# reflective_entity_duplicator

!!! info "v1.4.8"

Mirrors child props across a ship, such as bollards on both sides. The packed holders store no variables.

Find placed ones in the Scene panel with `s:reflective_entity_duplicator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a holder whose children are the props to mirror. Packed ships use `knobs_holder` for bollards and `attachment_machine_holder` for the attachment machine.

## What you set

Native panel. The four names that sit with this script are below. Every opened placement leaves them unwritten.

| Panel field | What to do with it |
|---|---|
| **Reflective Spawner** | Spawner control. Not stored on the packed holders. |
| **Show Panel** | Opens the tool panel. Not stored. |
| **Spawn** | Spawn control. Not stored. |
| **Reflect Position Axis** | Axis used when a child is mirrored. Not stored. |

## What it does

The holder keeps a mirrored copy of its children. On `knobs_holder` the children are `bollard_a` entities, placed on both sides of the hull. `attachment_machine_holder` uses the same script with an `attachment_machine_nested` child.

## Where it lives

Native script. The only copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No loose prefab and no `scene.xscene` stores it.

## Examples

**Bollards on both sides.** Packed prefab `knobs_holder`. The script has no variables. The first child is `bollard_a`. Seven holders use this script. Six of them are `knobs_holder`.

**Attachment machine.** Packed prefab `attachment_machine_holder` under `longship_ship_nested`. The script has no variables. The child is `attachment_machine_nested`.
