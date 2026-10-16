# barrier_builder

!!! info "v1.4.8"

Builds a barrier along a path. Height comes from **Z Scale**. **Mask** chooses who the barrier affects.

Find placed ones with `s:barrier_builder`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an empty entity and set **Path Name** to a path that is not already used. Set the height with **Z Scale**. A second barrier that reuses a path name is rejected.

## What you set

Native panel. Villa barriers store this order.

| Panel field | What to do with it |
|---|---|
| **Path Name** | Path to build along. Must be unique. |
| **Mesh** | Barrier mesh. Opened villa barriers store `barrier`. |
| **Z Scale** | Height. Villa uses `3`, `5`, `6`, and `7`. |
| **Flip** | Flips the barrier. |
| **Mask** | `Everything`, `Only Agents`, `Only AI Agents`, or `Only Missile and Melee`. |
| **Barrier** | Checkbox stored beside the mask. |
| **Ai Limiter** | Checkbox stored beside the mask. |
| **Agent Only** | Checkbox stored beside the mask. |
| **Missile Only** | Checkbox stored beside the mask. |

## What it does

The entity builds the mesh along **Path Name** at **Z Scale**. **Flip** reverses it. **Mask** is the filter. The four checkboxes are stored with it. If those mask flags disagree, they are reset.

## Where it lives

Native script. No prefab under the official modules stores it. Scenes place it directly.

## Examples

**Villa agent barriers.** `StoryMode\SceneObj\villa_singular_c`. Entity `barrier3`: path `barrier3`, mesh `barrier`, **Z Scale** `5`, **Flip** off, **Mask** `Only Agents`, **Barrier** off, **Ai Limiter** on, **Agent Only** on, **Missile Only** off. `barrier6` uses **Z Scale** `6`. `barrier7` uses `5`. `barrier_exit` uses path `barrier_exit` and **Z Scale** `3`.

**Flipped barrier.** Same scene, entity `barrier1`. **Flip** on, **Z Scale** `7`, path `barrier1`. The mask and the four checkboxes match `barrier3`.
