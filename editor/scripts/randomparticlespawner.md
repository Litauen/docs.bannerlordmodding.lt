# RandomParticleSpawner

!!! info "v1.4.8"

Every few seconds, restarts the instanced particles on one random child.

Find placed ones in the Scene panel with `s:RandomParticleSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a parent. Each child that should be able to fire needs an instanced particle system. Children without one are still eligible to be picked, and then nothing restarts.

## What you set

| Panel field | What to do with it |
|---|---|
| **spawnInterval** | Seconds between picks. Default `3`. |

## What it does

The timer starts at **spawnInterval**. When it hits zero, one child is chosen at random and every instanced particle on that child is restarted. The timer then waits **spawnInterval** again. The editor ticks it too.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. Nothing else in the game scripts calls it. The behavior above is the whole script.
