# ShipBridge

!!! info "v1.4.8"

The walkable bridge that appears between two ships after a [ShipAttachmentMachine](/editor/scripts/shipattachmentmachine/) connects. The class has no panel fields.

Find placed ones in the Scene panel with `s:ShipBridge`. See [Filter](/editor/editor/#filter).

## Where you put it

Do not place this script in the scene. When a hook connection finishes, the attachment builds the bridge entity and adds this script to it.

## What you set

The class declares no editor fields. No prefab stores a `<script name="ShipBridge">` block.

## What it does

The bridge entity is the parent of the plank meshes. The attachment adds four random connection planks on the source side and four on the target side, then adds this script. Agents crossing that bridge are tracked by the attachment's stepped-agent manager.

## Where it lives

Managed script, a nested mission object next to [ShipBridgeNavmeshHolder](/editor/scripts/shipbridgenavmeshholder/). It is created in code when the attachment state becomes connected.

## Examples

No scene file and no prefab stores this script.

**Runtime bridge.** The attachment instantiates `ship_connection_nav_mesh_plank`, fills it with connection planks, and adds `ShipBridge` to the bridge entity. The navmesh child of that plank prefab gets [ShipBridgeNavmeshHolder](/editor/scripts/shipbridgenavmeshholder/).
