# DeploymentPoint

!!! info "v1.4.8"

A siege deploy marker. It gathers the siege weapons inside **Radius**, or weapons tagged **SiegeWeaponTag**, and the deployment UI uses that set.

Find placed ones in the Scene panel with `s:DeploymentPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop `siege_deployment_placeholder_part_a` or `part_b` from `Native\Prefabs\props_siege.xml` on the ground in front of a wall or gate. Put it on the siege level.

## What you set

| Panel field | What to do with it |
|---|---|
| **Side** | Who deploys here. Code default `Attacker`. |
| **Radius** | Meters. A siege weapon closer than this is included. Code default `3`. |
| **SiegeWeaponTag** | Tag that also includes a weapon, on the weapon or its parent. Code default `dpWeapon`. The placeholder prefabs store `no_deploy`, so they collect by radius only. |

## What it does

After the mission starts, the point lists siege weapons under the tag or inside the radius. A ram makes it a ram point. A siege tower makes it a tower point and links the ladders on that tower's side. Anything else is a ranged point. If that list is empty, the point is hidden and marked as a breach. The breach path then looks for a siege tower on the same list.

## Where it lives

Managed script. The placeholders are in `Native\Prefabs\props_siege.xml`.

## Examples

**Wide attacker pad.** Prefab `siege_deployment_placeholder_part_a`. **Side** `Attacker`, **Radius** `7.750`, **SiegeWeaponTag** `no_deploy`.

**Narrow attacker pad.** Prefab `siege_deployment_placeholder_part_b`. **Side** `Attacker`, **Radius** `4.160`, **SiegeWeaponTag** `no_deploy`.

**Defender pad on a siege map.** `SandBox\SceneObj\sturgia_castle_siege_001`. One `siege_deployment_placeholder_part_b` sets **Side** to `Defender` and **Radius** to `4.960`. The tag stays the prefab's `no_deploy`. It is on levels `level_1` and `siege`.
