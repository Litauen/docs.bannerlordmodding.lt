# BlockadePositionScript

!!! info "v1.4.8"

Lays out the ship slots of a naval blockade as arcs between two child markers. The campaign map reads it when a port is under blockade.

Find placed ones in the Scene panel with `s:BlockadePositionScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the town entity on the campaign map, next to the town manager. The script reads two children tagged `Blockade_Arc_Start` and `Blockade_Arc_End`. The naval map names those entities `town_blockade_start` and `town_blockade_end`.

## What you set

`NavalDLC\SceneObj\Main_map` stores this order. **RefreshVisualization** is a button the scene does not write.

| Panel field | What to do with it |
|---|---|
| **MaximumNumberOfShips** | Cap on slots. Code default is `12`. `town_V5` stores `10`. `town_EW1` stores `8`. |
| **NumberOfArcs** | How many arcs share that cap. Both towns store `4`. |
| **DistanceBetweenShips** | Angle step between ships, in radians. Code default is a quarter turn. `town_V5` stores `1.555`. |
| **DistanceRandomizationOnArcs** | Random extra yaw on a slot. Both towns store `0.1`. Used only while randomization is on. |
| **DistanceRandomizationBetweenArcs** | Random extra radius between arcs. Both towns store `0.1`. |
| **Angle** | Sweep of the blockade, in radians. Code default is a right angle. `town_V5` stores `1.921`. |
| **MissionShipId** | Prefab dropped when ship visualization is refreshed. Both towns store `dromon_ship_nested`. |
| **ShipScaleFactor** | Scale of those preview ships. Code default is `0.052`. `town_V5` stores `0.045`. |
| **IsVisualizationEnabled** | On: the editor draws the arc while this entity is being edited. `town_V5` stores on. |
| **IsRandomizationEnabled** | On: slot positions jitter. `town_V5` stores off. |
| **IsShipVisualizationEnabled** | On: **RefreshVisualization** drops **MissionShipId** on each slot. `town_V5` stores off. |
| **RefreshVisualization** | Button. Rebuilds the arcs and, if ship visualization is on, spawns the preview ships. |

## What it does

The two tagged children are the ends of the arc. The script fits a circle through them using **Angle**, then walks slots along **NumberOfArcs**. The campaign asks for a slot list up to **MaximumNumberOfShips** when it shows a blockade at that town.

## Where it lives

Managed script. It is stored on town entities in `NavalDLC\SceneObj\Main_map`. No prefab stores it.

## Examples

**Vlandian port.** `NavalDLC\SceneObj\Main_map`, entity `town_V5` (tag `town`). **MaximumNumberOfShips** `10`, **DistanceBetweenShips** `1.555`, **Angle** `1.921`, **ShipScaleFactor** `0.045`, visualization on, randomization off.

**Empire port.** The same scene, entity `town_EW1`. **MaximumNumberOfShips** `8`, **DistanceBetweenShips** `1.545`. The arc count stays `4`.
