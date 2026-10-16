# DefencePoint

!!! info "v1.4.8"

A standing line for one battle side. Agents are meant to queue backward from this entity. No vanilla scene saves this script.

Find placed ones in the Scene panel with `s:DefencePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity. Face the entity the way the line should face. Slots step backward along that facing.

## What you set

| Panel field | What to do with it |
|---|---|
| **Side** | Which team may stand here. `Defender`, `Attacker`, or `None`. An unset value is `Defender`. |

## What it does

`GetVacantPosition` walks up to 100 slots behind the entity and returns the first slot that does not already contain an ally of **Side**. `AddDefender` and `RemoveDefender` keep the agent list on the script.

Castle gates and wall segments expose a defence-point list, and order code reads the first and last point on that list. In this version those lists are never filled, and nothing else calls `AddDefender`.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

**No placed copy.** The readers that were opened are the order controller and the tactic component. Both expect `DefencePoints` on a castle gate or a wall segment. The gate and wall scripts that were opened never assign that list.
