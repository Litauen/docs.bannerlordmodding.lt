# ShipAttachmentPointMachine

!!! info "v1.4.8"

The point a [ShipAttachmentMachine](/editor/scripts/shipattachmentmachine/) hooks, and the far end of the bridge once the ships are pulled together. The prompt is **Hook** until the bridge is down, then **Bridge**. The action is **Cut Loose**.

Find placed ones in the Scene panel with `s:ShipAttachmentPointMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity named `attachment_point_machine` on the ship. Give it a child named `hook_attach_point`, a child named `focus_object`, and children tagged `connection_point`, `connection_barrier`, `step_capsule`, and `bridge_target`.

The packed prefab is `attachment_point_machine_nested` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. `attachment_machine_holder` places copies of it. `attachment_machine_nested` also carries one.

## What you set

The nested point stores the usable-machine rows plus the offset, in this order.

| Panel field | What to do with it |
|---|---|
| **AmmoPickUpTag** | Tag of an ammo standing point. The nested point stores `ammopickup`. The hook point does not use one. |
| **NavMeshPrefabName** | Navmesh prefab to import. The nested point leaves it empty. |
| **WaitStandingPointTag** | Tag of a wait point. The nested point stores `Wait`. |
| **RelatedShipNavmeshOffset** | Navmesh id offset for this point. The nested prefab stores `14`. Holder copies store another integer. |
| **PilotStandingPointTag** | Tag of the standing point the crewman uses. The nested point stores `Pilot`. |

From `attachment_point_machine_nested`:

``` xml
<script name="ShipAttachmentPointMachine">
    <variables>
        <variable name="AmmoPickUpTag" value="ammopickup"/>
        <variable name="NavMeshPrefabName" value=""/>
        <variable name="WaitStandingPointTag" value="Wait"/>
        <variable name="RelatedShipNavmeshOffset" value="14"/>
        <variable name="PilotStandingPointTag" value="Pilot"/>
    </variables>
</script>
```

## What it does

While the bridge is down, a crewman at the pilot point can cut it loose. The standing point stays off until a bridge is connected and the ship is not being abandoned. The `focus_object` child is shown only while that point is usable. If the ship is abandoned, the pilot jumps clear.

## Where it lives

Managed script on a usable machine. Packed prefabs: `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml` (`attachment_point_machine_nested`, `attachment_machine_nested`, `attachment_machine_holder`).

## Examples

No scene file stores this script.

**Nested point, full row set.** Prefab `attachment_point_machine_nested`. Entity `attachment_point_machine`. Offset `14`.

**The same rows on the hook machine.** Prefab `attachment_machine_nested`. Entity `attachment_point_machine`. Offset `14`.

**A placed copy that only changes the offset.** Prefab `attachment_machine_holder`. Entity `attachment_point_machine` stores **RelatedShipNavmeshOffset** `13`. Another copy in that holder stores `5`. Some copies of `attachment_point_machine_nested` in the holder store no variables, so the nested prefab's rows stay.
