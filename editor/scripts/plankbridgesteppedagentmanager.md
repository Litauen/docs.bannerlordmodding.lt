# PlankBridgeSteppedAgentManager

!!! info "v1.4.8"

Counts agents standing on a ship boarding bridge and makes that stretch of navmesh more expensive as it gets crowded. No panel fields. No scene stores it.

Find placed ones in the Scene panel with `s:PlankBridgeSteppedAgentManager`. See [Filter](/editor/editor/#filter).

## Where you put it

You do not place it. When a ship attachment builds its bridge, it creates an empty entity named `Plank Bridge Physics` and adds this script.

## What you set

No fields.

## What it does

Agents on that bridge, within `5` m of the entity, are added into a total weight. Each agent also on the bridge navmesh face adds `7.5` to that face's cost, so later pathing prefers a clearer plank. The weight is cleared when the bridge resets the count.

## Where it lives

Managed script. Nothing under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC scenes or prefabs writes it. The ship attachment creates it at runtime.

## Examples

No placement. The runtime entity is `Plank Bridge Physics`, created with the bridge barriers when two ships link a plank.
