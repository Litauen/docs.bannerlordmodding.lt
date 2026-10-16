# ShipAttachmentMachine

!!! info "v1.4.8"

The hook a crewman uses to pull two ships together, then the bridge they cross. The prompt is **Hook** until the bridge is down, then **Bridge**.

Find placed ones in the Scene panel with `s:ShipAttachmentMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity named `attachment_machine` on the ship. Give it a child named `hook`, a child named `pile` with [RopePileBaked](/editor/scripts/ropepilebaked/), and a standing point tagged **PilotStandingPointTag**.

The packed prefab is `attachment_machine_nested` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. `attachment_machine_holder` places copies of it.

**RelatedShipNavmeshOffset** has to be unique among the machines on one ship.

## What you set

The packed machine stores the usable-machine rows plus the offset, in this order.

| Panel field | What to do with it |
|---|---|
| **AmmoPickUpTag** | Tag of an ammo standing point. Default `ammopickup`. The hook does not use one. |
| **NavMeshPrefabName** | Navmesh prefab to import. The nested machine leaves it empty. |
| **WaitStandingPointTag** | Tag of a wait point. Default `Wait`. |
| **RelatedShipNavmeshOffset** | Navmesh id offset for this machine. The nested prefab stores `14`. Copies store another integer. |
| **PilotStandingPointTag** | Tag of the standing point the crewman uses. Default `Pilot`. |

From `attachment_machine_nested`:

``` xml
<script name="ShipAttachmentMachine">
    <variables>
        <variable name="AmmoPickUpTag" type_name="string" value="ammopickup"/>
        <variable name="NavMeshPrefabName" type_name="string" value=""/>
        <variable name="WaitStandingPointTag" type_name="string" value="Wait"/>
        <variable name="RelatedShipNavmeshOffset" type_name="int" value="14"/>
        <variable name="PilotStandingPointTag" type_name="string" value="Pilot"/>
    </variables>
</script>
```

## What it does

A crewman at the pilot point throws the hook. While the ropes pull, the pile mesh is drawn toward the other ship’s attachment point. When the ships are close enough the bridge is shown. The action is **Use** until the bridge is connected, then **Cut Loose**. The description is **Hook**, then **Bridge**.

[ShipAttachmentMachineConnectionLogic](/editor/scripts/shipattachmentmachineconnectionlogic/) can drop a pulling rope and pick a better point on the same target ship.

## Where it lives

Managed script on a usable machine. Packed prefabs: `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml` (`attachment_machine_nested`, `attachment_machine_holder`).

## Examples

**Nested machine, full row set.** Prefab `attachment_machine_nested`. Entity `attachment_machine`. Offset `14`. Child `hook`. Child `pile` has `RopePileBaked` and mesh `sail_rope_pile_d`.

**A placed copy that only changes the offset.** Prefab `attachment_machine_holder`. Entity `attachment_machine` links the nested machine and stores **RelatedShipNavmeshOffset** `13`. The other four values stay on the nested prefab. Further machines in that holder use other offsets, including `4`, `5`, `8`, `9`, `12`, and `15`.
