# NavalPhysicsAgentCollisionSolver

!!! info "v1.4.8"

Pushes this floating body when a swimming agent hits its physics box. No panel fields. Put it on the same entity as **NavalPhysics**.

Find placed ones in the Scene panel with `s:NavalPhysicsAgentCollisionSolver`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the root that already has **NavalPhysics**. Loose debris in `floatable_props.xml` (`floatable_door_top`, `floatable_barrel`) has **NavalPhysics** and does not have this script. The packed debris prefabs do.

## What you set

No fields. Prefabs store the script with no variables.

## What it does

Each physics step it looks for agents in the water near the body's box. An agent whose eyes are inside the box shoves the body backward. An agent within `0.6` m of a box edge pushes and torques the body. Agents out of the water are ignored. The push uses the lighter of the body mass and the agent mass.

## Where it lives

Managed script. Placements are in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. One of them is prefab `floatable_debris_barrel_a`. No loose scene file stores the script.

## Examples

**Debris barrel.** Packed prefab `floatable_debris_barrel_a`. **NavalPhysics** and this script, no variables on either.

The same packed file repeats that pair on the other floatable debris roots. Each copy is the script with an empty variable list.
