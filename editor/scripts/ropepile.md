# RopePile

!!! info "v1.4.8"

Writes four points and a factor into the entity’s first mesh every frame. The mesh is `ropepiletestmesh`.

Find placed ones in the Scene panel with `s:RopePile`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that has the rope mesh. The two prefabs are `rope_pile` and `rope_pile_auto_curve` in `Modules\NavalDLC\Prefabs\rope_piles.xml`. The auto-curve prefab uses material `rope_pile_auto_curve_material`.

Ship grappling ropes use [RopePileBaked](/editor/scripts/ropepilebaked/), not this script.

## What you set

Both prefabs store the same values.

| Panel field | What to do with it |
|---|---|
| **point0** | First point written into the mesh bone frame. Default zero. |
| **point1** | Second point. Default zero. |
| **point2** | Third point. Default zero. |
| **point3** | Origin of that bone frame. Default zero. |
| **factor** | Written into the mesh vector’s Z. Default `0`. |

``` xml
<script name="RopePile">
    <variables>
        <variable name="point0" value="0.000, 0.000, 0.000, -1.000"/>
        <variable name="point1" value="0.000, 0.000, 0.000, -1.000"/>
        <variable name="point2" value="0.000, 0.000, 0.000, -1.000"/>
        <variable name="point3" value="0.000, 0.000, 0.000, -1.000"/>
        <variable name="factor" value="0.000"/>
    </variables>
</script>
```

## What it does

Each tick the first mesh gets one extra bone frame built from the four points, and **factor** is copied into the mesh vector. With the defaults the frame stays at the origin.

## Where it lives

Managed script. Prefabs: `Modules\NavalDLC\Prefabs\rope_piles.xml`.

## Examples

**Straight pile.** Prefab `rope_pile`. Mesh `ropepiletestmesh`. All four points and **factor** stay at zero.

**Auto-curve pile.** Prefab `rope_pile_auto_curve`. Same script values. The mesh material is `rope_pile_auto_curve_material`.

**Shipyard copy of the auto-curve pile.** `NavalDLC\SceneObj\khuzait_shipyard`. Entity `rope_pile_auto_curve` at the docks. The scene stores the same five zeros.
