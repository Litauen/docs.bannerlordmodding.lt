# ShipUnmannedOar

!!! info "v1.4.8"

An oar with no crew. It still strokes and retracts with the deck, and it stops when the oar is destroyed or the ship is sinking. The class has no panel fields. The stroke angles come from [ShipOarDeck](/editor/scripts/shipoardeck/).

Find placed ones in the Scene panel with `s:ShipUnmannedOar`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the oar entity. It needs a child tagged `oar_entity` (or named `oar`) and a retracted frame tagged `retracted_entity` (or named `retracted_frame`). Children must have **Does not Affect Parent's Local Bounding Box**.

Loose prefabs in `NavalDLC\Prefabs\ship_machines.xml`: `ShipOarLowDeckRight_4m`, `ShipOarLowDeckRight_6m`, `ShipOarLowDeckRight_8m`, `ShipOarLowDeckLeft_4m`, `ShipOarLowDeckLeft_6m`, `ShipOarLowDeckLeft_8m`. Packed names include `oar_left_lower_4m` and `oar_left_lower_6m`.

## What you set

The class declares no editor fields. Placements store an empty script tag.

## What it does

The oar mesh moves between the extracted frame and the retracted frame using the deck angles. It stops while the ship is abandoned, sinking, or the destructible on this entity is destroyed.

## Where it lives

Managed script. Loose prefabs in `NavalDLC\Prefabs\ship_machines.xml`. Packed copies in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Low-deck right, 6 m.** `NavalDLC\Prefabs\ship_machines.xml`, entity `ShipOarLowDeckRight_6m`. Empty script tag. Child `oar` is tagged `oar_gate_right` and uses mesh `oar_single_6m`.

**Low-deck left, 6 m.** Entity `ShipOarLowDeckLeft_6m` in the same file. Empty script tag. Child `oar` is tagged `oar_gate_left`.

**Packed lower oars.** Prefabs `oar_left_lower_4m` and `oar_left_lower_6m`. Empty script tag on the root entity of the same name.
