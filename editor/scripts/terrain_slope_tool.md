# terrain_slope_tool

!!! info "v1.4.8"

Shapes the terrain under an editor plane. Rise lifts the ground. Lower cuts it.

Find placed ones in the Scene panel with `s:terrain_slope_tool`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a scaled `editor_plane`. The plane's scale is the patch of ground. `mp_sergeant_map_018` uses one about `5.4 × 12.3`.

## What you set

Native panel. The names that sit together are below. Opened planes only write **Rise** and **Lower**.

| Panel field | What to do with it |
|---|---|
| **Apply** | Applies the slope. Opened planes do not store it. |
| **Selection Set** | Limits the edit to a selection set. Opened planes do not store it. |
| **Rise** | Lift terrain toward the plane. The sergeant-map plane stores on. |
| **Snap Terrain to Entity** | Match the terrain to this entity. Opened planes do not store it. |
| **Lower** | Cut terrain down to the plane. One sergeant-map plane stores off. Another stores on together with **Rise**. |

## What it does

The plane is the tool. **Rise** builds ground up to it. **Lower** digs ground down to it. A plane can store both on.

## Where it lives

Native script. No prefab stores it. The opened placement is an `editor_plane` in `Native\SceneObj\mp_sergeant_map_018`.

## Examples

**Raise only.** `Native\SceneObj\mp_sergeant_map_018`. Entity `editor_plane` at about `(701, 650, 7)`, scale about `5.4 × 12.3 × 2.5`, mesh `editor_plane_low`. **Rise** on, **Lower** off.

**Raise and cut.** Same scene, a second `terrain_slope_tool` stores **Rise** on and **Lower** on.
