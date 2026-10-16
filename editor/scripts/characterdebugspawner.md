# CharacterDebugSpawner

!!! info "v1.4.8"

Editor preview that spawns one character on this entity and walks them through a fixed list of actions. No vanilla scene saves this script.

Find placed ones in the Scene panel with `s:CharacterDebugSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity in an editor scene. The character is a child visual, not a mission agent.

## What you set

| Panel field | What to do with it |
|---|---|
| **PoseAction** | Action played when the visual is created. Default `act_walk_idle_unarmed`. Changing it restarts that action. |
| **LordName** | Character id to spawn. Default `main_hero`. Changing it rebuilds the body, skeleton, and equipment from that character. |
| **IsWeaponWielded** | When on, the visual wields the first weapon and the first off-hand item in that character's equipment. |

## What it does

After the editor game data has loaded, the script spawns **LordName** once. It then picks a random direction and speed and, about once a second, switches the visual to another action from its built-in list (conversation poses, jumps, sits, throne idles). Removing the entity clears the visual.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

**No placed copy.** Nothing in those modules writes the script. The only behavior is the editor tick on the script itself: spawn **LordName** (default `main_hero`) and keep changing its action.
