# sail_visual

!!! info "v1.4.8"

Folds, unfolds, and burns a sail, and rolls a lateen yard. The dropdown name is `sail_visual`. The script picker files it under `ship_visual_only`.

Find placed ones in the Scene panel with `s:sail_visual`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the sail root. Packed sails are `lateen_sail_large_nested`, `lateen_sail_medium_nested`, `lateen_sail_small_nested`, `sail_square_large_nested`, `sail_square_medium_nested`, and `sail_square_small_nested` in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

The root expects a mast, a yard, a cloth mesh, and a child tagged `pulley_systems_parent` full of [pulley_system](/editor/scripts/pulley_system/) rigs. Square sails also have `lvl1`, `lvl2`, and `lvl3` cloth sets.

## What you set

Field order is the class order. Packed files store the same names in a shuffled order. Buttons are on the panel and are not stored.

| Panel field | What to do with it |
|---|---|
| **Fold Sail Duration** | Seconds to fold. Class default `3`. Packed sails store `5`. |
| **Folded Sail Transition Duration** | Seconds to blend into the folded mesh. Class default `0.5`. Packed sails store `0.2`. |
| **Fold Free Bone Reset Duration** | Seconds to park the free bones. Class default `1.2`. Packed sails store `0.25`. |
| **Unfold Sail Duration** | Seconds to open. Class default `4`. Packed sails store `2`. |
| **Fold Sail Step Multiplier** | Fold speed scale. Class default `2`. Lateen packs store `2`. Square packs store `0.5`. |
| **Lateen Yard Shift** | How far a lateen yard slides. Class default `0`. Large lateen stores `0.77`. Medium and small lateen store `0.74`. Square sails store `0`. |
| **Lateen Roll Change Degree Limit** | Degrees before a lateen yard flips. Packed sails store `20`. |
| **Lateen Roll Change Animation Duration** | Seconds for that flip. Packed sails store `3`. |
| **Lateen Roll Change Animation Step Multiplier** | Flip speed scale. Packed sails store `1`. |
| **Lateen Roll Change Yard Shift Start** | When the yard starts sliding, in the flip. Packed sails store `3`. |
| **Lateen Roll Change Yard Shift Duration** | Seconds of that slide. Packed sails store `4`. |
| **Lateen Roll Change Yard Shift Acceleration** | Slide accel. Packed sails store `8`. |
| **Lateen Roll Degrees** | Roll angle. Lateen packs store `45`. Square packs store `0`. |
| **Rope Connection Max Distance** | How far a rope may search for a knot. Packed sails store `7`. |
| **Knob Type** | `Bollard`, `Cleat`, or `Belaying`. Packed sails store `Bollard`. |
| **Place Knobs** | Button. The class does not read it. Packed sails do not store it. |
| **Knob Color** | Color used with that button. Packed sails store white `1, 1, 1`. |
| **Start Fire** | Button. The class does not read it. Packed sails do not store it. |
| **Place Cloth Fragments** | Button. The class does not read it. Packed sails do not store it. |
| **Sail Type** | `SquareSail` or `LateenSail`. Lateen packs store `LateenSail`. Square packs store `SquareSail`. |
| **Burning Animation Duration** | Seconds of the burn. Large lateen stores `40`, medium `25`, small `20`. Large square stores `35`, medium `20`. |
| **Square Lvl3 Mast Shift** | Mast shift for the third square-sail set. Lateen packs store `0`. Large square stores `2.1`, medium `1.5`. |
| **Editor Only Level Selection** | `None`, `Lvl1`, `Lvl2`, or `Lvl3`. `None` follows the ship. Packed sails store `None`. |
| **Top Lateen Fire Material** | Material on the upper lateen while it burns. Square packs store `square_sail_fire`. Lateen packs store empty. |
| **Editor Only Ship Health** | Health the editor uses for the burn, from `0` to `1`. Packed sails store `1`. |
| **Top Flag Rope Position** | Flag height along the rope, `0` to `1`. Class default `0.8`. Packed sails do not store it. |
| **Capture Flag Bottom Rope Position** | Lower height the flag drops to. Class default `0.25`. Packed sails do not store it. |
| **Start Capture The Flag Animation** | Button. The class does not read it. Packed sails do not store it. |

## What it does

**Sail Type** picks the rig. Square sails swap `lvl1`, `lvl2`, and `lvl3` from the ship, unless **Editor Only Level Selection** forces one. Lateen sails roll the yard by **Lateen Roll Degrees** and slide it by **Lateen Yard Shift**.

Fold time is **Fold Sail Duration** plus the bone reset and the folded blend. Unfold time is **Unfold Sail Duration**. Ropes under `pulley_systems_parent` are pulled to match.

The flag sits at **Top Flag Rope Position** and can drop toward **Capture Flag Bottom Rope Position**. **Editor Only Ship Health** is the health value the editor burn reads.

## Where it lives

Managed script. Only `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No loose prefab and no `scene.xscene` stores it.

## Examples

**Large lateen.** Packed entity `lateen_sail_large_nested`. **Sail Type** `LateenSail`, yard shift `0.77`, roll `45`, fold duration `5`, burn `40`, level `None`.

**Medium lateen.** Packed entity `lateen_sail_medium_nested`. Same type, yard shift `0.74`, burn `25`.

**Large square.** Packed entity `sail_square_large_nested`, tag `sail_center_0`. **Sail Type** `SquareSail`, roll `0`, step multiplier `0.5`, level-3 mast shift `2.1`, burn `35`, fire material `square_sail_fire`.
