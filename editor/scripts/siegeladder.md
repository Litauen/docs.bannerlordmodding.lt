# SiegeLadder

!!! info "v1.4.8"

The ladder attackers lift onto a wall and defenders push back with a fork. The description is **Ladder** on the lift and push points, and **Fork** on the ammo point. The actions are **Lift**, **Push**, and **Pick Up**. Scenes place a [SiegeLadderSpawner](/editor/scripts/siegeladderspawner/), which spawns this prefab.

Find placed ones in the Scene panel with `s:SiegeLadder`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the spawner in a siege scene. The weapon prefabs are in `Native\Prefabs\props_siege_ladders.xml` and are hidden in the prefab browser (`hide_in_prefab_editors`). Heights are `siege_ladder_6m`, `siege_ladder_7m`, `siege_ladder_8m`, `siege_ladder_11m`, and `siege_ladder_14m`, plus an `_mp` copy of each.

The body needs children for the tags below: `ladderDown`, `ladderUp`, `ladder_body`, `ladder_collision_body`, and `initialwaitposition`. Attacker points are tagged `attacker`. Defender points are tagged `defender`. Fork stands use `right`, `left`, and `front`.

## What you set

`siege_ladder_6m` stores this order. The `8m`, `11m`, and `14m` prefabs swap the animation prefix. `siege_ladder_7m` keeps the `6m` clips.

| Panel field | What to do with it |
|---|---|
| **AttackerTag** | Attacker stand tag. Prefab: `attacker`. |
| **DefenderTag** | Defender stand tag. Prefab: `defender`. |
| **downStateEntityTag** | Child for the ladder on the ground. Prefab: `ladderDown`. |
| **IdleAnimation** | Idle clip. `6m` and `7m`: `siege_ladder_6m_idle`. `8m`: `siege_ladder_8m_idle`. `11m`: `siege_ladder_11m_idle`. `14m`: `siege_ladder_14m_idle`. |
| **_idleAnimationIndex** | Cached clip index. Prefab stores `-1`. |
| **RaiseAnimation** | Raise clip. Same height prefix, `_rise`. `7m` uses `siege_ladder_6m_rise`. |
| **RaiseAnimationWithoutRootBone** | Raise clip without the root bone. Suffix `_rise_wo_rootbone`. |
| **_raiseAnimationWithoutRootBoneIndex** | Cached clip index. Prefab stores `-1`. |
| **PushBackAnimation** | Push-back clip. Suffix `_push_back`. |
| **_pushBackAnimationIndex** | Cached clip index. Prefab stores `-1`. |
| **PushBackAnimationWithoutRootBone** | Push-back without the root bone. Suffix `_push_back_wo_rootbone`. |
| **_pushBackAnimationWithoutRootBoneIndex** | Cached clip index. Prefab stores `-1`. |
| **TrembleWallHeavyAnimation** | Heavy stop against the wall. Suffix `_stop_wall_heavy`. |
| **TrembleWallLightAnimation** | Light stop against the wall. Suffix `_stop_wall_light`. |
| **TrembleGroundAnimation** | Heavy stop on the ground. Suffix `_stop_ground_heavy`. |
| **RightStandingPointTag** | Right fork stand. Prefab: `right`. |
| **LeftStandingPointTag** | Left fork stand. Prefab: `left`. |
| **FrontStandingPointTag** | Front fork stand. Prefab: `front`. |
| **PushForkItemID** | Fork item. Prefab: `push_fork`. |
| **upStateEntityTag** | Child for the ladder on the wall. Prefab: `ladderUp`. |
| **BodyTag** | Ladder body. Prefab: `ladder_body`. |
| **CollisionBodyTag** | Collision body. Prefab: `ladder_collision_body`. |
| **InitialWaitPositionTag** | Wait position. Prefab: `initialwaitposition`. |
| **LadderPushTreshold** | Push needed to move the ladder. Code default `170`. Prefab stores `175`. |
| **LadderPushTresholdForOneAgent** | Push from one agent. Code default `55`. Prefab stores `60`. |
| **BarrierTagToRemove** | Barrier removed when the ladder is up. Prefab: `barrier`. The spawner can replace this. |
| **initialState** | `OnLand` on every ladder prefab. |
| **IndestructibleMerlonsTag** | Merlons that stay when the ladder arrives. Prefab stores empty. The spawner can fill this. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty. |
| **AddOnDeployTag** | Tag shown on deploy. Empty. The spawner copies its own value here. |
| **PilotStandingPointTag** | Pilot stand tag. Prefab: `Pilot`. |
| **AmmoPickUpTag** | Fork pickup tag. Prefab: `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. Prefab: `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. Empty. |

## What it does

Attackers at the `attacker` points lift the ladder. Defenders at the `defender` points push it back. The fork point picks up `push_fork`. The spawner copies side, wall segment, on-wall navmesh, both rotation angles, the barrier tag, and the merlon tag onto this weapon.

## Where it lives

Managed script. Prefabs in `Native\Prefabs\props_siege_ladders.xml`. No scene file stores the weapon script. Siege scenes store the spawner.

## Examples

**6 m ladder.** `siege_ladder_6m`. Idle `siege_ladder_6m_idle`. Push thresholds `175` and `60`. **initialState** `OnLand`. The `6m_mp` copy uses the same clips.

**7 m ladder, 6 m clips.** `siege_ladder_7m`. Idle is still `siege_ladder_6m_idle`. The `7m_mp` copy matches that.

**Taller ladders.** `siege_ladder_8m`, `siege_ladder_11m`, and `siege_ladder_14m` use the matching height prefix on every animation row. Each has an `_mp` copy with the same clips.
