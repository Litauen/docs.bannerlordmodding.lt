# SpawnedItemEntity

!!! info "v1.4.8"

A weapon lying in the scene that an agent can pick up. Missions usually add the script when a weapon is dropped or a missile sticks. The editor prefab is an empty stand-in.

Find placed ones in the Scene panel with `s:SpawnedItemEntity`. See [Filter](/editor/editor/#filter).

## Where you put it

The runtime copy is created on the weapon entity. The only authored prefab is `spawned_item` in `Native\Prefabs\editor_helpers_&_tests.xml`.

## What you set

That prefab stores this order. It also stores `IsDroppedItem` and `ObjectInteractionDistance`. Those names are not fields on the class.

| Panel field | What to do with it |
|---|---|
| **WeaponName** | Item id for an authored weapon. The prefab leaves it empty. Dropped weapons fill the weapon in code. |
| **DescriptionMessage** | Focus description. The prefab leaves it empty. A live weapon uses the item name. |
| **ActionMessage** | Focus action. The prefab leaves it empty. A live weapon uses Equip, Fill, or Swap. |
| **IsInstantUse** | Instant use. The prefab stores off. |

## What it does

The player can equip the weapon, fill a partial stack, or swap it with a held weapon. A banner uses the banner pickup path. Missiles and dropped gear are created with this script already on the entity.

## Where it lives

Managed script. Prefab `spawned_item` is in `Native\Prefabs\editor_helpers_&_tests.xml`. No scene file stores the script. Mission weapon spawn creates it at runtime.

## Examples

**Empty editor prefab.** `spawned_item`. **WeaponName** empty, **IsInstantUse** off. The same block stores `IsDroppedItem` false and `ObjectInteractionDistance` `3`, which the class does not declare.

No scene placement. The caller is mission weapon spawn: a dropped weapon, a stuck missile, or a weapon attached to a corpse gets this script and is initialized with that weapon.
