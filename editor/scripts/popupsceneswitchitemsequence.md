# PopupSceneSwitchItemSequence

!!! info "v1.4.8"

Swaps one equipment slot on the notification character when a sequence state fires. An empty item clears that slot.

Find placed ones in the Scene panel with `s:PopupSceneSwitchItemSequence`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an entity whose tag is the **name** of the `PopupSceneSpawnPoint` for that character. The spawn point passes its character visual into the sequence.

No vanilla scene stores it. Weapons that are already in hand at the start belong on the spawn point’s wielded-item fields.

## What you set

| Panel field | What to do with it |
|---|---|
| **InitialItem** | Item id applied on the initial state. Empty clears the slot. |
| **PositiveItem** | Item id applied on the positive state. |
| **NegativeItem** | Item id applied on the negative state. |
| **InitialBodyPart** | Slot for the initial item. |
| **PositiveBodyPart** | Slot for the positive item. |
| **NegativeBodyPart** | Slot for the negative item. |
| **InitialActivationTime** | Seconds before the initial swap. Default `0`. From the sequence base. |
| **PositiveActivationTime** | Seconds before the positive swap. Default `0`. |
| **NegativeActivationTime** | Seconds before the negative swap. Default `0`. |

**Body part** values: `None`, `Weapon0`, `Weapon1`, `Weapon2`, `Weapon3`, `ExtraWeaponSlot`, `Head`, `Body`, `Leg`, `Gloves`, `Cape`, `Horse`, `HorseHarness`. `None` skips the swap. Default is `None`.

## What it does

When the timer fires, the item is written into that equipment slot and the character visual is refreshed with the right hand wielding slot `0`. An empty item name clears the slot.

The notification calls the initial state on open and the positive state when the player continues. It does not call the negative state.

## Where it lives

Managed script, on the sequence base. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The caller is `PopupSceneSpawnPoint`: entities tagged with the spawn point’s name receive the character visual and the state changes. For a weapon that is already equipped when the shot opens, set **RightHandWieldedItem** or **LeftHandWieldedItem** on the spawn point instead. `scn_execution_notification` does that with `execution_axe`.
