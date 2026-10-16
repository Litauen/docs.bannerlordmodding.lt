# MissionShipRam

!!! info "v1.4.8"

The ram on a ship. It is a capsule on the bow that damages another ship when this one hits it.

Find placed ones in the Scene panel with `s:MissionShipRam`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ram mesh, under the ship. The ship is the parent that owns the script. The mesh names in vanilla are `ram_01` through `ram_08`.

## What you set

The packed prefab writes the fields in this order. Length, radius, and the offset are the ones that change per ram. Code defaults, if you add the script with no variables, are length `5`, radius `0.5`, offset `0, 0, 0`, and damage multiplier `1`.

| Panel field | What to do with it |
|---|---|
| **_ramAttachmentPointOffset** | Local offset of the capsule start, in meters. Vanilla rams sit about 14–15 m forward and about half a meter down. |
| **NavMeshPrefabName** | Left empty on every vanilla ram. |
| **_ramLength** | Capsule length. Vanilla rams use about `3.75` to `5.1`. |
| **_ramRadius** | Capsule radius. Vanilla rams use about `0.39` to `0.63`. |
| **_ramTierDamageMultiplier** | Scales ram damage. Every vanilla ram stores `1`. |

`ram_02` in the packed prefab:

``` xml
<variable name="_ramAttachmentPointOffset" value="0.000, 14.600, -0.570"/>
<variable name="_ramLength" value="4.500"/>
<variable name="_ramRadius" value="0.460"/>
<variable name="_ramTierDamageMultiplier" value="1.000"/>
```

## What it does

On mission start the capsule is pushed onto the ship body. A fast hit toward the middle of another ship deals ram damage, knocks people at the contact down, and leaves a broken-plank decal (`decal_ship_damaged_b_heap`). A deep hit can stick the two hulls until they turn apart or the target is wrecked. The ram does not collide with other ships as a normal body; the capsule is the hit.

## Where it lives

Managed script. The only copies are in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`, prefab `ram_group`. No loose scene file stores the script.

## Examples

**Short heavy ram.** `ram_01`. Offset `0, 14.8, -0.56`, length `3.75`, radius `0.57`.

**Mid ram.** `ram_02`. Offset `0, 14.6, -0.57`, length `4.5`, radius `0.46`.

**Longer ram.** `ram_04`. Offset `0, 14.8, -0.5`, length `5.1`, radius `0.48`.

**Wide ram.** `ram_08`. Offset `0, 15.4, -0.35`, length `4.25`, radius `0.634`.

Later ship entities in that same packed file instance these rams and do not write new numbers.
