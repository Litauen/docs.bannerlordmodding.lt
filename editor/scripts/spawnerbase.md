# SpawnerBase

!!! info "v1.4.8"

The shared base of the siege spawners. It only chooses which prefab to spawn. A scene places a derived spawner, such as [SiegeTowerSpawner](/editor/scripts/siegetowerspawner/), and does not place this script.

Find placed ones in the Scene panel with `s:SpawnerBase`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it only if you want the two override names and nothing else. The siege spawners already include these rows. No vanilla prefab or scene stores `SpawnerBase`.

## What you set

| Panel field | What to do with it |
|---|---|
| **ToBeSpawnedOverrideName** | Prefab name to spawn. Default empty. Empty means the entity name with the last `_piece` removed. |
| **ToBeSpawnedOverrideNameForFireVersion** | Prefab spawned when a derived spawner asks for the fire version. Default empty. Empty means the stone name plus `_fire`. |

## What it does

On mission start a derived spawner creates the prefab from those two names. Tags on the spawner entity are copied onto the spawned machine. The spawned entity is not saved back into the scene.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores `SpawnerBase`.

## Examples

No placed entity. [SiegeTowerSpawner](/editor/scripts/siegetowerspawner/) is one derived spawner. With the override empty, `siege_tower_9m_spawner` spawns `siege_tower_9m`. That spawner does not request a fire version, so the fire override stays unused there.
