# Custom Creature: the skeleton

Getting a creature's rig right, and why the rig you have in Blender is probably not the rig the
engine will use.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. If you only need to rig a **human**
mesh, [Armature/Skeleton](/3d/armature_skeleton/) already covers that and this page is not for you.

!!! note "Version"
    Measured against **Bannerlord v1.4.8**.

## The one rule

**Author against the skeleton the engine will play the clip on. Never against a mesh FBX.**

Here is why, and it is not obvious.

Skinning uses bind matrices, and bind matrices are **roll-independent**. A mesh FBX can carry
arbitrary bone orientations and still deform perfectly in game. Rotations are **not**
roll-independent.

So this inference is false:

> The mesh works in game with vanilla animations, therefore its rig is correct.

It proves the bone **positions** and the weights are right. It says nothing at all about
orientations. An animation authored on that rig can look flawless in Blender and come out twisted
in the engine, and **nothing in Blender will ever show you the problem.**

TAOM lost a full day and six wrong diagnoses to this on a single head-butt clip.

### What the difference actually looks like

`horse_skeleton`, read out of the engine's own asset:

```
32 bones, no *_nub_notused entries
horsepelvis -> horsespine1 -> horsespine2 -> horsespine3 -> horseneck1 -> horseneck2 -> horse_head
```

Compared against a mesh FBX that skins to that same skeleton and animates correctly in game:

| Fact | The engine | The mesh FBX |
|---|---|---|
| `horseneck1`'s parent | `horsespine3` | `horsetail3` |
| Bone axis | X along the bone | Blender imports it Y along the bone |
| `_nub_notused` bones | none | 7, which the Kit drops on import |

The mesh rig parents the neck to a **tail** bone. It skins fine anyway. It animates wrong.

## Getting the real skeleton

Open `Modules/Native/AssetPackages/skeletons.tpac` in [TpacTool](/resources/tpactool/). Every
vanilla skeleton is in that one file, with bone names, parents and rest frames.

!!! warning "Do not give up on the first package that fails"
    The per-creature rig packages do **not** parse: `pack_horse_customrig`,
    `animations_horse_and_rider`, `animations_movement_and_behaviour`, `pack_anim_cutscene`,
    `animation_clips.tpac` and `Assets.tpac` all throw `Frames not equal` or
    `capacity was less than the current size`. A day was lost treating that as "the skeleton is
    unobtainable". `skeletons.tpac` holds them all and works.

If you would rather script it, TAOM has a PowerShell wrapper over `TpacTool.Lib` that dumps a
skeleton to JSON, which is convenient if you want to rebuild the rig in Blender programmatically:

```
pwsh dump_engine_skeleton.ps1 -List
pwsh dump_engine_skeleton.ps1 -Skeleton horse_skeleton -OutFile horse_skeleton.json
```

It also takes `-TpacToolBin` and `-NativeDir`, and its defaults are hardcoded to one machine, so
set both to your own TpacTool `bin` folder and your `Modules/Native` folder.

You do not need any of this. TpacTool's own window shows you the same data.

### Rest-frame maths, if you are rebuilding the rig in Blender

`RestFrame` is **row-vector** form: the rows are the basis vectors and `M41..M43` is the offset.
Blender is column-vector. Transpose the 3x3 and move the offset to the last column:

```python
Matrix(((m[0], m[3], m[6], m[9]),
        (m[1], m[4], m[7], m[10]),
        (m[2], m[5], m[8], m[11]),
        (0.0,  0.0,  0.0,  1.0)))
```

Then accumulate down the hierarchy (`world = parent_world @ local`) and build each bone with
`head = world.translation`, `tail = head + world_X * length`, `eb.align_roll(world_Z)`.

Skip the transpose and every bone offset reads zero, every bone stacks on the root, and the rig
collapses to a point. That bug survived a while because the only check being run was an fcurve
**count**, never a visual one.

!!! danger "Knowing the engine stores bones X-along does NOT mean exporting with `primary_bone_axis='X'`"
    This inference is natural, it was made, and it was tested. It produced the worst result of the
    entire session: the creature folded in on itself.

    The convention the engine **stores** rest frames in and the Blender export setting that
    reproduces a clip the Kit reads correctly are two different questions, and knowing the first
    does not answer the second. Export with `primary_bone_axis='Y'`, `secondary_bone_axis='X'`.

## Bone limits

There is exactly one, and it is not the one you may have read:

**`Skeleton.MaxBoneCount = 64`. It is a cap on TOTAL bones per skeleton, not per mesh.** Author 63
or fewer for margin.

!!! note "There is no per-mesh bone palette limit"
    A widely-repeated "~40 bones per mesh / per draw call" figure does not exist. TAOM published it
    internally for months and it was wrong. Two in-game measurements killed it: a war elephant
    renders as **one** mesh skinned to 59 active bones, and a two-horse chariot renders as **one**
    mesh skinned to 54.

    This matters because the false limit is used to justify splitting a creature body into several
    meshes, and splitting causes its own problems. A fully disjoint body mesh in
    `<AdditionalMeshes>` may not render at all.

**Keep the whole body in one mesh.** Split only for a genuinely separate sub-mesh with a different
simulation need. The warg splits its fur so the fur can cloth-simulate independently, which is
cloth-driven, not bone-driven.

## Exporting the rig

Skeleton and meshes go in **one FBX**. A mesh-only export (armature deselected) drops the skin
weights entirely, because Blender stores skin in the FBX armature deformer. Re-import such a file
and you will find zero vertex groups.

```python
object_types={'ARMATURE', 'MESH'}
primary_bone_axis='Y', secondary_bone_axis='X'
axis_forward='-Y', axis_up='Z'
add_leaf_bones=False
bake_anim=False
```

For a **mesh** export the armature keeps its real name. (The `<skeleton>_notused` rename is for
animation-only exports, and is covered on [the animation page](/guides/custom_creature_animation/).)

!!! warning "`primary_bone_axis` is not a preference, and the obvious test will not catch it"
    At `X`/`Y` the bone **heads** and the bounding box stay correct to 1e-06 while the **tails**
    drift over half a metre, putting a hip bone 88% of its reference motion out of place. A
    validation gate that checks head positions or bounding boxes passes the broken file.

    **Check tails.**

Also note `primary_bone_axis='X'` force-aligns every bone to world X and destroys mirror symmetry
on a symmetric rig.

After import, set the skeleton's **Type** in the Kit. A fresh import comes in as `other`; quadruped
mounts want `horse`.

## Two things in the Kit that look like failures and are not

**A duplicate name error is the protection working.** If you import a second FBX containing an
armature the module already has, you get:

```
Unable to import skeleton_warg(Skeleton). Item with same name already exists in
Warg_Rig_V5_geo. Asset names are required to be unique within the same module.
```

That is correct. The Kit enforces per-module name uniqueness, skips the duplicate, imports the
meshes, and lets them bind to the skeleton already present. No workaround is needed. Renaming the
armature to dodge it produces a tpac with **no** Skeleton item, which merely looks like success.

**The bone-octahedron display is cosmetic.** With `add_leaf_bones=False` the Kit computes bone tails
itself and draws them scrambled, even on data that is perfectly symmetric. Bannerlord uses only each
bone's origin and rotation for skinning; it does not use tails. **Judge by the model viewer, never
by the skeleton editor.**

## Package the skeleton with the mesh

Every working creature bundles its Skeleton item **inside** the same `.tpac` as its meshes.

!!! danger "Do not ship a standalone skeleton-only tpac"
    It is an unproven structure and it crashed the game: a recursive worker-thread native access
    violation reading null, on spawn. That is strictly worse than the problem it was trying to fix.

If a mesh re-export silently drops the Skeleton item, the symptom is quiet: the `action_set` still
declares `skeleton="<name>"`, that now resolves to nothing, agent-skeleton creation returns null,
and **the rider spawns with no mount at all**. No crash, no error. Easy to miss for days.

Check after every mesh re-export that the Skeleton item is still in the live tpac, and compare
against a known-good creature rather than against your own expectations.

## Textures: four rules the Kit does not enforce loudly

None of these produces a clear error message.

**Dimensions must be divisible by 4, and should be a power of two.** Block compression works on 4x4
blocks. Eight maps at 5689x5689 (odd, so not divisible by 4) silently fell back to uncompressed
`R8G8B8A8_UNORM` at **164 MiB each**. Six of them were essentially the entire 940 MB runtime cache
payload for one creature. The same map at 2048 with DXT5 and mips is 5.33 MiB.

The formula worth memorising:

```
bytes = width x height x bpp x 4/3
```

where `4/3` is the mip chain and `bpp` is 4.0 uncompressed, 1.0 for BC3/DXT5 and BC5, 0.5 for BC1
and BC4. **Halving the dimension quarters the memory.**

**Palette-mode PNGs compile to single-channel BC4.** A single-channel normal map is worthless.
Re-save as RGB. Watch out that a resize pass will silently skip files already at the target size, so
they keep their palette: the de-palette has to be a separate forced re-encode.

**Uppercase in a texture filename can crash the editor** when the texture is assigned to a material.
Mesh names are lowercased automatically on import (`SK_GD_Fellwarg` becomes `sk_gd_fellwarg`) while
texture names keep their case, and that asymmetry is exactly the shape that produces a lookup miss.
Lowercase is safe either way.

**Force colour management off on data maps.** Normal, AO and metallic are data, not colour. Set
`colorspace_settings.name = 'Non-Color'` and the view transform to Standard at gamma 1.0 before
saving, or you get a creature with subtly wrong lighting that nobody can explain later.

## Materials do not survive an FBX re-import

An FBX carries only the material slots it actually has. Anything assigned by hand in the editor
exists **only** inside the compiled tpac, and a re-import silently loses it.

One TAOM rig carried three material slots for five meshes; four more had been editor-assigned by the
original author. After re-import the creature had every XML row, every material asset and every clip
correct, and still did not appear in battle.

!!! warning "A missing material binding does not present as a missing material"
    It presents as **the wrong colour, or as nothing at all.** Bindings sit one per LOD level, so a
    creature can render correctly in a close-up UI preview and be invisible in the world, where it
    draws at a lower LOD.

Re-assign materials in the Kit after any re-import, and check every LOD.

## Physics: bodies and ragdoll joints

A freshly imported skeleton comes in with `Usage = 'other'`, every collision body empty
(`body_type='none'`, mass 0, radii at an unset sentinel) and **zero** joint constraints. An FBX
re-import rebuilds the bone definition but not this data.

A healthy creature skeleton has typed bodies and roughly one constraint per bone. For comparison:

| Skeleton | Bones | Usage | Ragdoll constraints |
|---|---|---|---|
| `human_skeleton` | 28 | `human` | |
| `horse_skeleton` | 32 | `horse` | |
| `skeleton_warg` | 49 | `horse` | 48 |
| `elephant_skeleton` | 60 | `horse` | 59 |
| `spider_skeleton` | 62 | `other` | |

Zero constraints and every body typed `none` means the data was dropped. Setting it by hand in the
Kit is real work: for a 58-bone creature that is 57 joints times roughly 12 fields each.

## Next

[Authoring the animation clips](/guides/custom_creature_animation/), or, if you took the reskin
path and need no clips, straight to [the XML](/guides/custom_creature_xml/).
