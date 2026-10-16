# RopePileBaked

!!! info "v1.4.8"

The rope mesh a ship hook pulls toward another ship. The attachment machine moves it. The script has no fields of its own.

Find placed ones in the Scene panel with `s:RopePileBaked`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the child named `pile` under a [ShipAttachmentMachine](/editor/scripts/shipattachmentmachine/). The mesh is `sail_rope_pile_d` with material `plank_stake_a_rope_pile_baked`, or the test mesh `ropepiletestmesh`.

## What you set

The script class adds no fields. The test prefab `rope_pile_baked` in `Modules\NavalDLC\Prefabs\ship_machines.xml` still stores three rows:

| Stored name | What the prefab writes |
|---|---|
| **Position1** | `(0, 0, 0)` |
| **Position2** | `(7, 7, 0)` |
| **Progress** | `0.5` |

The packed ship piles store the script with no variables. The battle code does not read those three names. It updates the mesh from the hook and the target point.

## What it does

The attachment machine finds this script on a descendant and drives the rope mesh. A straight pull draws the rope from the source to just short of the target. A thrown hook draws the falling curve. The mesh bounding box grows with the rope and resets when the hook is idle.

## Where it lives

Managed script. Test prefab: `Modules\NavalDLC\Prefabs\ship_machines.xml` (`rope_pile_baked`). The ship copies are the child `pile` inside `attachment_machine_nested` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Test rope.** Prefab `rope_pile_baked`. Mesh `ropepiletestmesh`, material `rope_pile_baked_material`. Stores **Position1**, **Position2**, and **Progress** as above.

**Hook rope on the attachment machine.** Packed prefab `attachment_machine_nested`. Child entity `pile`. Script stored with no variables. Mesh `sail_rope_pile_d`, material `plank_stake_a_rope_pile_baked`.

**A placed machine that reuses that pile.** Packed prefab `attachment_machine_holder`. The child `pile` links the same rope script and the same mesh.
