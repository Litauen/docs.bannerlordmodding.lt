# PopupSceneEmissionHandler

!!! info "v1.4.8"

Eases a shader vector on every child mesh, so a glow can come up after a delay in a notification scene.

Find placed ones in the Scene panel with `s:PopupSceneEmissionHandler`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty parent. The meshes that change are the children.

## What you set

| Panel field | What to do with it |
|---|---|
| **startTime** | Seconds before the ease starts. The one vanilla placement stores `15`. |
| **transitionTime** | How long the ease takes, in seconds. That placement stores `5`. |

## What it does

After **startTime**, each child mesh’s vector argument is eased over **transitionTime**. The fourth component goes from `0` to `10`. The editor runs the same tick, so you can watch it in the scene.

## Where it lives

Managed script. One scene stores it.

## Examples

**Burned wood after a death in war.** `SandBox\SceneObj\scn_cutscene_family_member_death_war`. Entity `empty_object`. **startTime** `15`, **transitionTime** `5`. One child is the mesh `burned_wood_e`.
