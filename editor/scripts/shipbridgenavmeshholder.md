# ShipBridgeNavmeshHolder

!!! info "v1.4.8"

The navmesh strip agents walk while crossing a ship bridge. The class has no panel fields. It is a separate script from [ShipBridge](/editor/scripts/shipbridge/).

Find placed ones in the Scene panel with `s:ShipBridgeNavmeshHolder`. See [Filter](/editor/editor/#filter).

## Where you put it

Do not place this script in the scene. When a hook connection finishes, the attachment instantiates `ship_connection_nav_mesh_plank`, finds the child tagged `navmesh_holder`, and adds this script to that child.

## What you set

The class declares no editor fields. No prefab stores a `<script name="ShipBridgeNavmeshHolder">` block.

## What it does

On connect it imports the navmesh prefab `ship_connection_plank_navmesh_1` at the next free navmesh id, and attaches five face groups from that id. The two middle groups are moved every tick so the strip follows the gap between the ships. The stepped-agent manager on the attachment reads this holder.

## Where it lives

Managed script, a nested mission object in the same file as [ShipBridge](/editor/scripts/shipbridge/). The plank entity it is added to is the prefab `ship_connection_nav_mesh_plank`.

## Examples

No scene file and no prefab stores this script.

**Runtime holder.** The attachment creates `ship_connection_nav_mesh_plank`, takes the child tagged `navmesh_holder`, adds `ShipBridgeNavmeshHolder`, and passes the new navmesh id plus the source attachment machine.
