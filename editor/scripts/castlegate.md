# CastleGate

!!! info "v1.4.8"

A castle gate that opens, closes, takes ram and melee hits, and breaks. The same entity is the destructible door and the point defenders hold.

Find placed ones in the Scene panel with `s:CastleGate`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the gate entity. Outer gates are tagged `outer_gate`. Inner gates use `inner_gate`. The door skeleton uses **LeftDoorBoneName** and **RightDoorBoneName**. Extra colliders use **ExtraCollisionObjectTagLeft** and **ExtraCollisionObjectTagRight**. Children tagged `middle_pos` and `wait_pos` are the defender positions. Children tagged `open` and `close` are the use points.

## What you set

`Native\Prefabs\props_siege_gates_outer.xml` stores this order on `aserai_castle_gate_outer_l1`.

| Panel field | What to do with it |
|---|---|
| **OwningTeam** | Who owns the gate. This gate stores `Defenders`. |
| **OpeningAnimationName** | Open clip. This gate stores `aserai_castle_gate_outer_l1_opening`. |
| **ClosingAnimationName** | Close clip. This gate stores `aserai_castle_gate_outer_l1_closing`. |
| **HitAnimationName** | Ram hit clip. This gate stores `aserai_castle_gate_outer_l1_hit`. |
| **PlankHitAnimationName** | Plank hit clip. This gate stores `aserai_castle_gate_outer_l1_plank_hit`. |
| **HitMeleeAnimationName** | Melee hit clip. This gate stores `aserai_castle_gate_outer_l1_hit_melee`. |
| **DestroyAnimationName** | Break clip. This gate stores `aserai_castle_gate_outer_l1_break`. |
| **NavigationMeshId** | Face id of the doorway. This gate stores `462`. Code default is `1000`. |
| **NavigationMeshIdToDisableOnOpen** | Face id turned off while the gate is open. This gate stores `106`. Code default is `-1`. |
| **LeftDoorBoneName** | Left door bone. This gate stores `bn_middle_l`. Code default is `bn_bottom_l`. |
| **RightDoorBoneName** | Right door bone. This gate stores `bn_middle_r`. Code default is `bn_bottom_r`. |
| **ExtraCollisionObjectTagRight** | Extra collider on the right. This gate stores `extra_collider_r`. |
| **ExtraCollisionObjectTagLeft** | Extra collider on the left. This gate stores `extra_collider_l`. |
| **ActivateExtraColliders** | On: those extra colliders are used. This gate stores on. |
| **SideTag** | Which side of the castle this gate is. This gate stores `middle`. A ram spawner uses the same tag. |
| **AutoOpen** | On: the gate opens on its own. This gate stores off. |
| **PilotStandingPointTag** | Pilot stand tag. This gate stores `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. This gate stores `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. This gate stores `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. This gate leaves it empty. |

The ram-hit sound is `event:/mission/siege/door/hit`.

## What it does

Defenders and attackers use the open and close points. **OpeningAnimationName** and **ClosingAnimationName** drive the door bones. A ram hit, a plank hit, and a melee hit each play their clip. Destruction plays **DestroyAnimationName**. **NavigationMeshId** is the doorway agents path through. **NavigationMeshIdToDisableOnOpen** is blocked while the leaves are open. **SideTag** is how a [BatteringRamSpawner](/editor/scripts/batteringramspawner/) picks this gate.

## Where it lives

Managed script. Outer, inner, and sally gate prefabs are in `Native\Prefabs\props_siege_gates_outer.xml`, `props_siege_gates_inner.xml`, and `props_siege_gates_sally.xml`. Town and castle scenes place those prefabs.

## Examples

**Aserai outer gate, level 1.** `Native\Prefabs\props_siege_gates_outer.xml`, entity `aserai_castle_gate_outer_l1`, tag `outer_gate`. Clip names are the `aserai_castle_gate_outer_l1_*` set. Doorway face `462`, face disabled while open `106`, bones `bn_middle_l` and `bn_middle_r`, **SideTag** `middle`.

**Empire outer gate, level 3.** The same file, entity `empire_castle_gate_outer_l3`. **OpeningAnimationName** is `empire_castle_gate_outer_l3_opening` and **ClosingAnimationName** is `empire_castle_gate_outer_l3_closing`.
