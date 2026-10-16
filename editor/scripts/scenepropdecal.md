# ScenePropDecal

!!! info "v1.4.8"

Builds a unique material for this entity’s meshes from a decal material plus four textures, tiling, and a sweep. No vanilla scene places it.

Find placed ones in the Scene panel with `s:ScenePropDecal`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that has the decal mesh. Every mesh on the entity gets the generated material. The first mesh also gets the tiling and sweep.

## What you set

| Panel field | What to do with it |
|---|---|
| **DiffuseTexture** | Diffuse map. Empty skips it. |
| **NormalTexture** | Normal map. Empty skips it. |
| **SpecularTexture** | Specular map. Empty skips it. |
| **MaskTexture** | Second diffuse map, and the material flag `use_areamap`. Empty skips it. |
| **UseBaseNormals** | On: the mesh vector’s fourth sweep value is `1`. Off: `0`. Default off. |
| **TilingSize** | Tile repeat on X and Y. Default `1`. |
| **TilingOffset** | Tile offset on X and Y. Default `0`. |
| **AlphaTestValue** | Alpha-test cutoff on the material. Default `0`. |
| **TextureSweepX** | Sweep X. Default `0`. |
| **TextureSweepY** | Sweep Y. Default `0`. |
| **MaterialName** | Material to copy. Default `deferred_decal_material`. |

Editing any field rebuilds the material. The editor does this on init as well.

## What it does

The script copies **MaterialName**, assigns whichever textures are set, and writes tiling onto the first mesh. **UseBaseNormals** and the two sweep numbers share that mesh’s second vector.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. There is no other caller. A placed decal in vanilla scenes is a mesh or a [ScenePropNegativeLight](/editor/scripts/scenepropnegativelight/), not this script.
