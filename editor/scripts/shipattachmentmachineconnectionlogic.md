# ShipAttachmentMachineConnectionLogic

!!! info "v1.4.8"

While a hook’s ropes are still pulling, swaps that hook onto a clearer attachment point on the same target ship. No panel fields.

Find placed ones in the Scene panel with `s:ShipAttachmentMachineConnectionLogic`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ship root, the entity that has the ship physics. It finds the ship on that root and walks that ship’s attachment machines.

## What you set

The script adds no fields. The packed ship prefabs store it with an empty variable list.

## What it does

Each tick, for a machine whose ropes are pulling and that is allowed to look for a better point, it scores the other free attachment points on the target ship. If one scores clearly higher, the current link is destroyed and the hook connects to that point.

It does not run once the bridge is already down.

## Where it lives

Managed script. Packed ship prefabs in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. It sits next to `NavalPhysics` on the hull.

## Examples

Each of these stores the script with no variables, on the ship root.

**Cog.** `cog_ship_nested`.

**Drakkar.** `drakkar_ship_nested`.

**Dhow.** `dhow_ship_nested`.

**Dromon.** `dromon_ship_nested`.

**Galley.** `galley_ship_nested`.

**Roundship used by the story quest.** `crusas_roundship_nested_q5`.

**Burning storyline cog.** `burning_cog_ship_nested_storyline`.

The same empty script is on `corbita_ship_nested`, `fishing_ship_nested`, `catship_ship_nested`, and the storyline copies of the fishing ship, catship, dromon, and galley.
