# WorkshopAreaMarker

!!! info "v1.4.8"

Marks one workshop slot in a town. **AreaIndex** is the slot. **AreaRadius** is how far it claims shop props and the sign.

Find placed ones with `s:WorkshopAreaMarker`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop prefab `workshop_1`, `workshop_2`, or `workshop_3`. Each is tagged `workshop_area_marker`. The town handler only keeps markers with that tag. Props tagged `shop_prop` plus `a`, `b`, `c`, `d`, `e`, or `f` inside the radius are the frames for that workshop's six prop meshes. An entity tagged `shop_sign` inside the radius gets the workshop sign mesh.

## What you set

| Panel field | What to do with it |
|---|---|
| **AreaRadius** | Meters around the entity. Prefab default `4`. The area-marker class default is `3`. |
| **AreaIndex** | Workshop slot. Prefabs use `1`, `2`, and `3`. The name lookup reads `Workshops[AreaIndex]` and ignores `0`. |
| **CheckToggle** | Area-marker helper. Opening the entity in the editor clears it. Prefabs do not store it. |

## What it does

**AreaIndex** selects the town workshop used for the sign and for the prop meshes. Slot `1` is `Workshops[1]`. Entities tagged `shop_prop` and one of `a` through `f` inside the radius are replaced by that workshop type's prop mesh for that letter. The sign mesh is applied to `shop_sign` in the same radius.

## Where it lives

Managed script on the area-marker base. Prefabs are in `SandBox\Prefabs\sp_editor_spawnpoints.xml`.

## Examples

**Three town slots.** `sp_editor_spawnpoints.xml`. `workshop_1` stores **AreaRadius** `4` and **AreaIndex** `1`. `workshop_2` is index `2`. `workshop_3` is index `3`. All three are tagged `workshop_area_marker` and are on levels base, 1, 2, 3, and civilian.

**Town radii.** `SandBoxCore\SceneObj\empire_town_a`. Prefab `workshop_3` overrides **AreaRadius** to `26.550` and does not write **AreaIndex**. `workshop_1` stores `30.870`. `workshop_2` stores `25.690`.
