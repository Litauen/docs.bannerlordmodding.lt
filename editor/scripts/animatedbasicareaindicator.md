# AnimatedBasicAreaIndicator

!!! info "v1.4.8"

An area marker with a name and an icon id. In a town or village it claims the usable machines inside its radius. No vanilla scene saves this script.

Find placed ones in the Scene panel with `s:AnimatedBasicAreaIndicator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity at the spot you want marked. **AreaRadius** is the reach, in meters. **AreaIndex** becomes the tag `area_marker_` plus that number. Two markers with the same index share one tag.

## What you set

The editor shows the public fields. Nothing in vanilla writes them into a prefab, so these are the code defaults.

| Panel field | What to do with it |
|---|---|
| **AreaRadius** | Meters around the entity. Default `3`. Usable machines inside this distance are the ones the marker claims. |
| **AreaIndex** | Number in the tag `area_marker_0`, `area_marker_1`, and so on. Default `0`. |
| **CheckToggle** | While checked, the editor draws helpers. Opening the scene in the editor clears it. |
| **NameStringId** | Game-text id for the name. Empty means no name until code sets one. |
| **Type** | Icon id. The floating-fortress battle sets it to `quest`. The town marker pass that turns an empty type into the common-area icon reads `BasicAreaIndicator`, not this script. |

## What it does

In a town or village, usable machines inside **AreaRadius** are taken off their old spawn tag and grouped under `area_marker_{AreaIndex}`. A player conversation point inside the radius is grouped the same way.

The town name-marker pass draws `BasicAreaIndicator`, not this script. The one vanilla caller is the floating-fortress battle. It creates the script at runtime on an empty entity over each enemy ballista, sets the name to **Target**, **Type** to `quest`, and **AreaRadius** to `1.5`, then moves that entity with the weapon.

## Where it lives

Managed script, on the area-marker base. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

**Enemy ballista target.** Not a placed scene entity. During the floating-fortress mission the code adds this script to a new empty entity, name **Target**, type `quest`, radius `1.5`, and keeps the entity on the ballista until that weapon is destroyed.
