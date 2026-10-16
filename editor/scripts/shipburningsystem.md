# ShipBurningSystem

!!! info "v1.4.8"

Spreads fire across a ship's rails, deck, upgrades, and masts, and raises the fire lights as the fire grows. The editor tag is `ship_visual_only`.

Find placed ones in the Scene panel with `s:ShipBurningSystem`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the fire rig, with children tagged `railing_parent`, `ship_deck_parent`, `deck_upgrade_parent`, `mast_parent`, `light_parent`, and `sound_parent`. Each fire point under those parents is a [BurningNode](/editor/scripts/burningnode/). Sound children are [BurningSoundNode](/editor/scripts/burningsoundnode/).

The packed prefab is `ship_burning_system` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No scene file stores the script.

## What you set

The full copy of `ship_burning_system` stores seven rows. **Start Fire** and **Stop Fire** are editor buttons the prefab does not write. Panel order follows the class.

| Panel field | What to do with it |
|---|---|
| **Start Fire** | Editor button. Starts the spread in the editor. The prefab does not write it. |
| **Stop Fire** | Editor button. The prefab does not write it. |
| **Spread Rate** | How fast fire moves to the next node. The full prefab stores `1`. Other copies store `3`, `4`, or `5`. |
| **Fire Start Random Count** | How many nodes start already burning. The full prefab stores `2`. Other copies store `3`, `4`, or `5`. |
| **All Fire Mode** | On: fire lights stay visible before the fire starts. The full prefab stores off. |
| **Small Hit Debug** | Editor debug flag. The full prefab stores off. |
| **Min Fire Progress For Light** | Average fire amount before lights turn on. The full prefab stores `0.500`. A faster copy stores `0.100`. |
| **Max Fire Progress For Light** | Average fire amount at full light. The full prefab stores `1`. A faster copy stores `0.500`. |
| **Max Light Intensity** | Light strength at full fire. The full prefab stores `5000`. Other copies store `4000`. |

## What it does

A fire hit on the ship that deals more than `40` fire damage registers a blow on the nearest nodes. When the ship's fire points are gone, the system starts and the sound nodes start with it. Fire walks along each chain. A node whose position is under the water is put out for `3` seconds. Lights stay hidden until the average progress passes **Min Fire Progress For Light**, then their intensity climbs to **Max Light Intensity**.

## Where it lives

Managed script. Packed prefab `ship_burning_system` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Full row set.** Prefab `ship_burning_system`, entity `ship_burning_system`. Spread `1`, random start count `2`, all-fire mode off, light range `0.500` to `1`, max intensity `5000`.

**A faster copy.** The same prefab, another entity `ship_burning_system`. Spread `5`, random start count `4`, light range `0.100` to `0.500`, max intensity `4000`.
