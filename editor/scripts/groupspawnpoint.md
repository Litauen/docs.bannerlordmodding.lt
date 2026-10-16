# GroupSpawnPoint

!!! info "v1.4.8"

A usable place with a delay and a spawn count on the panel. No official scene or prefab places it, and no mission reads those two values.

Find placed ones with `s:GroupSpawnPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an entity you want agents to use as a standing place. It does not add its own mesh. Standing points are child usables, the same way as any other usable machine.

## What you set

| Panel field | What to do with it |
|---|---|
| **Delay** | Seconds. Default `-1`. Zero, or any negative value, means the use is instant. |
| **SpawnCount** | Default `1`. Nothing in the mission code reads this value. |

## What it does

The script only stores **Delay** and **SpawnCount**. Instant use is derived from **Delay**: negative (the default) or about zero. **SpawnCount** is not used by hideout spawning. Hideout reinforcements use a different marker, `StealthAreaMarker`, and a child tagged `reinforcement_ally_group_spawn_point_tag`.

## Where it lives

There is no prefab and no scene.

## Examples

No scene or prefab contains `GroupSpawnPoint`. The script is only the panel fields above.
