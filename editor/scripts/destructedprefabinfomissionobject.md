# DestructedPrefabInfoMissionObject

!!! info "v1.4.8"

Old companion to the retired destruction script. It stores a broken-prefab name and an offset. Nothing in the current game reads those fields.

Find placed ones in the Scene panel with `s:DestructedPrefabInfoMissionObject`. See [Filter](/editor/editor/#filter).

## Where you put it

Do not add it. Broken pieces for a live object belong on `DestructableComponent`, in **DestructionStates**.

## What you set

No vanilla prefab stores these. The defaults are the code values.

| Panel field | What to do with it |
|---|---|
| **DestructedPrefabName** | Prefab name. Default empty. |
| **Translate** | Offset. Default `0, 0, 0`. |
| **Rotation** | Euler offset. Default `0, 0, 0`. |
| **Scale** | Default `1, 1, 1`. |

## What it does

The class only holds those four values. No other managed script reads them.

## Where it lives

Managed script, marked obsolete. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

**No placed copy.** There is no scene example and no runtime caller. The class was opened and it has no init, no tick, and no method that spawns **DestructedPrefabName**.
