# LadderQueueManager

!!! info "v1.4.8"

Holds the queue of soldiers waiting to climb a siege ladder or a siege-tower ladder. The ladder or the tower overwrites the face id when the mission starts.

Find placed ones with `s:LadderQueueManager`. See [Filter](/editor/editor/#filter).

## Where you put it

Put one copy on the ladder for each side that queues. A siege ladder carries two, on the ladder root next to `SiegeLadder`. A siege tower puts one on each ladder child (`ladder_left`, `ladder_right`, `ladder_middle`).

You do not place the waiting spots by hand. The ladder builds the queue arc at mission start.

## What you set

| Panel field | What to do with it |
|---|---|
| **ManagedNavigationFaceId** | Navmesh face the queue is attached to. Ladder prefabs store `0`. Tower ladders store `6` or `7`. The mission replaces this. |
| **ManagedNavigationFaceAlternateID1** | Extra face. Prefabs store `0`. |
| **ManagedNavigationFaceAlternateID2** | Extra face. Prefabs store `0`. |
| **CostAddition** | Extra path cost. Prefabs store `0`. |
| **NavMeshPrefabName** | Navmesh for this entity. Prefabs leave it empty. |

## What it does

At mission start a siege ladder assigns the attacker queue and the defender queue. The attacker queue holds up to 3 climbers on the wall face. The defender queue holds 1 and blocks further use. A siege tower assigns the faces from its own dynamic navmesh (the `+5`, `+6`, and `+7` faces, and a separate `+2` queue) and sets how many soldiers may wait.

Until that runs, the saved face id is only the prefab value.

## Where it lives

`Modules\Native\Prefabs\props_siege_ladders.xml` and `Modules\Native\Prefabs\props_siege_siegetowers.xml`.

## Examples

**Two queues on a ladder.** `siege_ladder_6m` has two `LadderQueueManager` scripts, both with face id `0`, alternate ids `0`, and **CostAddition** `0`. The same pair is on `siege_ladder_7m`, `siege_ladder_8m`, `siege_ladder_11m`, and `siege_ladder_14m`.

**Tower ladders.** On `siege_tower_9m`, `ladder_right` and `ladder_left` store face id `7`. On `siege_tower_12m`, `ladder_left` stores `6`, `ladder_right` stores `7`, and `ladder_middle` stores `7`. `siege_tower_5m` does not carry this script.

No scene file stores this script. Scenes place the ladder or tower prefab.
