# Custom Creature: the animation clips

Authoring locomotion and attack clips for a creature, exporting them so the Modding Kit reads them
correctly, and the one tag whose absence crashes the game everywhere at once.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. Read
[the skeleton page](/guides/custom_creature_skeleton/) first: if you author against the wrong rig,
nothing on this page will save you.

!!! note "Version"
    Measured against **Bannerlord v1.4.8**.

## Locomotion clips are authored in place

A walk cycle has **zero net root travel**. The feet cycle, the body stays at the origin.

The engine supplies the translation itself, from the movement system. If you bake root motion into a
walk, the engine adds its translation to yours and the creature moves at double speed while its feet
skate.

`anf_displace_position` is not root motion from your keys: it moves the agent by the vector in the
clip's **displacement** clip usage, up to that usage's end progress, and without the usage the engine
reads null. Vanilla sets it on 337 clips, all with that usage, such as `death_fall_front`. **Keep it
off walks and runs:** their travel is the `bip_mov_ik` or `quad_movement` usage's loop displacement.
See [the clip inspector](/guides/custom_creature_clip_inspector/#clip-usages).

## `quad_movement`, or: the six-hour crash

This is the single most expensive lesson on these pages, so it goes near the top.

**Every gait clip in a `movement_system="quadrupedal"` action set must carry the `quad_movement` clip
usage.** Walks, runs, strafes, turns in motion, jumps. TAOM's reverse engineering of v1.5.3 found ten
places that read it with no null check. Step points make the footsteps (with `make_walk_sound`); they
are not shown to be needed against the crash, and 42 of the 142 vanilla clips with `quad_movement` have
none.

Without it:

1. The clip compiles fine. No error.
2. It even plays correctly on a detached, non-mount agent. So it tests clean.
3. Then a quadrupedal action set measures it, builds a **null** native gait structure, and the first
   `Skeleton.TickAnimations` or `GetWalkSpeedLimitOfMountable` dereferences it.
4. Access violation at offset `+0x10`, in **every** mount context at once: the inventory thumbnail, the
   character tableau, and mission deployment. That offset was seen on v1.4.x; the v1.5.3 offset has not
   been observed.

The secondary fingerprint, if you are staring at a log: resolving an unbound action through the
poisoned set returns a runtime-synthesised garbage name, something shaped like
`1002467048434979358_0`.

!!! danger "In the Modding Kit, `quad_movement` is a CLIP USAGE, not a Flag"
    It is in the collapsed **Clip usages** section, below the Flags list. People look for a checkbox
    in Flags, do not find one, and conclude the tag does not exist.

    `make_walk_sound` **is** a Flag. Step points are a third, separate field: they are the footstep
    timing fractions, and unset they read as `-1, -1, -1, -1`.

Attack, hit and death clips correctly do **not** carry `quad_movement`. Only movement clips.

### Per-category recipe

A working quadruped's clips on its own skeleton, by category:

| Clip category | Flags | Clip usages |
|---|---|---|
| gait (walk, run, gallop, turn, strafe) | `make_walk_sound` | **`quad_movement`**, plus step points for footsteps |
| attack | `client_prediction`, `lock_movement`, `enforce_all` | none |
| death | `make_bodyfall_sound`, `client_prediction`, `do_not_keep_track_of_sound`, `enforce_all`, `update_bounding_volume` | none |
| rear | `lock_movement`, `enforce_lowerbody` | none |

Gallop runs no longer get `cyclic`: vanilla's carry only `make_walk_sound`, and no native requirement
was found. A clip on the **horse** rig should copy vanilla's horse recipe:

| Vanilla clip | Priority | Flags | Blend in / out |
|---|---|---|---|
| `horse_kick` (attack) | 34 | `enforce_lowerbody`, `enforce_all` | 0.2 / 0.4 |
| `horse_rear` | 74 | `lock_movement`, `enforce_lowerbody`, `update_bounding_volume`, `ignore_slope` | 0.3 / 0.3 |
| `horse_hit_from_front` | 2 | `enforce_lowerbody` | 0.2 / 0.4 |

**Every vanilla horse clip read carries `enforce_lowerbody`.** Why priority decides what shows in battle:
[the clip inspector](/guides/custom_creature_clip_inspector/#priority-why-a-clip-plays-in-the-viewer-and-not-in-battle).

## Gait theory, or: why it looked wrong when everything was technically correct

Three things that made TAOM's creatures stop looking uncanny.

**Elephants use a four-beat lateral sequence** (left hind, left fore, right hind, right fore). Never
a trot, never a pace. Their mass means there is no aerial phase at all, so the "fast" gait is a
faster amble, not a different gait. Animating an elephant like a large horse reads as wrong
immediately and it is hard to say why.

**Spiders use an alternating tetrapod.** Four legs down, four legs moving, alternating sets.

**A spider idle is a braced, splayed stance, not a walk-in-place.** An idle built by damping a walk
looks like an animal treading water.

Amplitude edits have to be anchored to the **planted-frame** pose, or the stance foot floats.
Phase shifts are a cyclic time offset and are safe to apply freely.

## Exporting the clip

Armature only, and the armature object **and its data** are both renamed `<skeleton>_notused`.

```python
object_types={'ARMATURE'}
add_leaf_bones=False
primary_bone_axis='Y', secondary_bone_axis='X'
axis_forward='-Y', axis_up='Z'
bake_anim=True
bake_anim_use_all_bones=True
bake_anim_use_nla_strips=False
bake_anim_use_all_actions=False
bake_anim_force_startend_keying=True
bake_anim_step=1.0
bake_anim_simplify_factor=0.0
```

What shipped clips from three separate creatures agree on, and what they do not:

| Property | Verdict |
|---|---|
| UpAxis / FrontAxis / CoordAxisSign = `2 / 1 / -1` | **match this** |
| Model node types: `Null` 1 + `LimbNode` | match this |
| Bones the Kit drops on import | **must be 0** |
| TimeMode | **not load-bearing.** Shipped clips use both 24 and 30 fps |
| Clip flags count | **not load-bearing.** One shipped attack clip has zero flags and works |

Two traps in the export settings themselves:

* `bake_anim_use_all_bones=False` does **not** reduce the exported bone set. Blender bakes the whole
  armature whenever `bake_anim` is on. The flag name is misleading.
* Any `*_nub_notused` bone in the animation FBX is a difference from every shipped clip. Strip them
  before authoring.

!!! tip "Byte size is a reliable empty-bake detector"
    A healthy clip export is megabytes and takes seconds. An export that produced no keyframes comes
    out at roughly 0.06 MB in under 0.01 s. If your file is tiny, the bake found nothing.

    On recent Blender this is usually because renaming edit bones does **not** rewrite slotted-action
    fcurve data paths. Lowercase your bones and the fcurves still point at the old names, so the
    bake is empty.

## Compiling in the Kit

Import as a **Skeleton Animation**, set its **Owner Skeleton**, then create an **Animation Clip**:
**Source 1 = 1** and Source 2 = the master's last frame, because frame 0 is the rest frame
([the export mapping](/guides/custom_creature_animation/#the-export-mapping)); Duration must come out
above zero. Set Loading Type to Always keep in memory (0) on any clip you bind: the conservative
choice, not a proven requirement
([why](/guides/custom_creature_clip_inspector/#the-fields-one-by-one)). Keep the name to **63
characters** (the Kit warns `Could not set fixed-size(64) string`). Every field:
[the clip inspector](/guides/custom_creature_clip_inspector/).

!!! warning "A new clip starts at priority 0 with no flags, and may never show in battle"
    The model viewer plays it unopposed; in battle, by TAOM's reading, locomotion takes the channel
    back. TAOM's war ram logged 1,300 head-butts at priority 0 and 1,005 at 34 with no visible head
    drop; it now copies `horse_kick`'s flags and blends too, not yet seen in battle. Copy the nearest
    vanilla clip's whole recipe and judge it in a battle.

!!! danger "Do not rename a clip inside the Modding Kit"
    Renaming corrupts it. The Kit keeps resolving the old name, the inspector reports
    `Size in KB = 0`, it refuses to save, the model viewer draws a scrambled pose, and the renamed
    file can vanish outright. Restarting the tools clears that state, but nobody has recorded
    checking a clip renamed in the Kit afterwards: **no rename inside the Kit, with or without a
    restart, is known to give a usable clip.**

    Instead: create the clip on the default `new_animation_clip` name, set its source range and
    flags, save and **close the Kit**. Then rename the clip **item** inside its `_anm.tpac`, not just
    the file: the game registers a clip by that stored name. TAOM's optional
    [rename_anim_clip_tpac.py](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/rename_anim_clip_tpac.py)
    does it, keeping the item's GUID, and writes `<name>_anm.tpac`; a hex editor can make the same
    two-field edit ([the byte layout](/guides/custom_creature_clip_inspector/#clip-names)). Reopen
    the Kit.

The clip name itself does not need a particular prefix form. Bare names, single-prefixed and
double-prefixed all work; the `<skeleton>|` prefix you see on compiled clips comes from the Kit's
Owner Skeleton field, not from your take name.

## Riders are not the creature skeleton

A mounted rider is the standard **28-bone `human_skeleton`**. It is not your creature's rig.

Mounted rider poses are authored as human clips and bound in the `as_human_warrior` action set under
`act_<mount>_*` names mapping to `rider_<mount>_*` clips. At runtime the engine parents the rider to
the mount's `rider_sit_bone`.

Sit bones from shipped creatures, as a sanity check on your own:

| Mount | `rider_sit_bone` |
|---|---|
| horse | `horsespine2` |
| warg | `Spine1_M` |
| spider | `chest_m` |
| elephant | ` Spine1_05` |

!!! warning "That leading space in the elephant's bone name is real"
    Bone names exported from some tools carry leading spaces, and the XML must reproduce them
    exactly. A bone name that does not match resolves to index **-1**, silently, and a -1
    `rider_sit_bone` seats the rider at the world origin. The symptom is "my rider is floating at
    his own feet".

You do not have to author rider clips at all if an existing creature's fit close enough. TAOM's
spider reuses the warg's `rider_warg_*` clips.

## Diagnosing a clip that looks right in Blender and wrong in game

Do these in order. Each is cheap and eliminates a whole class.

1. **Read the compiled master back** in TpacTool and compare each bone's rotation with the FBX's local
   and the engine's rest local. If limbs play another limb's motion, match each slot at frame 0 to the
   bone whose rest it equals; a mismatch means the node order is wrong
   ([the export mapping](/guides/custom_creature_animation/#the-export-mapping), point 4).
2. **Check frame 0 is the rest pose** when the limbs move right but the body floats or the feet skate.
3. **Render a FRONT view.** A yaw is almost invisible from the side, and it is very easy to spend
   hours looking only at side views. Assert on bone **direction** vectors, not head positions.
4. **Count the bones the Kit will drop.** Any `*_nub_notused` is a difference from every shipped
   clip.
5. **Diff the FBX globals** against a shipped clip: UpAxis, FrontAxis, CoordAxisSign, node types.
6. **Compare the rig against the engine skeleton.** Parenting and bone axis are the two things a
   mesh FBX gets wrong with no visible symptom.

!!! warning "A verification that cannot fail is not a verification"
    Three hypotheses were "refuted" during one TAOM session by tests structurally incapable of
    detecting the fault, and two turned out to be real problems.

    Deleting the `_nub_notused` bones in Blender and seeing no pose change proves nothing: the nubs
    sit coincident with their child carrying identity rotation, so removing them is a no-op **by
    construction**. Re-parenting a bone in Blender and seeing no change fails the same way. And a
    bone's head position cannot detect a rotation about its own origin, so head checks are blind to
    exactly the error being hunted.

    Before trusting a negative result, ask what the test would show if the hypothesis were **true**.
    If the answer is "the same thing", the test is worthless.

## Pick the right reference creature

Compare your creature mount against a **single-creature mount**: a warg, a spider, an elephant.

Do not use a multi-creature rig such as a chariot as your reference. A two-horse-plus-cart rig is
its own skeleton asset whose horse-**named** bones are not the horse skeleton, and they sit 90
degrees off every mesh rig. That looks exactly like a smoking gun and sent two rounds of TAOM's work
in the wrong direction.

Measured against same-shaped creatures, a creature's animation FBX uses the **same** bone convention
as its own mesh FBX: median differences of 6.79 and 16.83 degrees, which are rest-pose differences,
not a 90 degree flip.

## The export mapping

Measured by reading Kit-compiled masters back against the FBX and the engine's rest frames, on
`human_skeleton` (52 clips) and `horse_skeleton`:

1. **The Kit stores an FBX's bone-local transforms as they are**, with no axis conversion. So the
   armature you export from must carry the engine's bone frames: each bone's `matrix_local` equal to
   the accumulated rest frame (`tail = head + Y column`, `align_roll(Z column)`). The bones then draw
   sideways in Blender, harmlessly. Export with `primary_bone_axis='Y'`, `secondary_bone_axis='X'`.
2. **The root is stored as its FBX world pose turned 180 degrees about Z,** with the armature object's
   transform applied on top. Keep the object at identity and bake the 180 degree turn into the pose.
3. **Frame 0 is the rest frame.** The root position track is stored relative to it, and every vanilla
   master opens on rest with its clip at Source 1 = 1. Open on a posed frame and the pelvis offset is
   lost: a hunched character stands too high and its feet skate. Key rest at frame 0, motion from
   frame 1 (the site's [animation notes](/modding/animations/#feet-above-the-ground) agree). Vanilla
   walks carry one root track, the pelvis bob; drop a pack's root travel and give it to the engine as
   the `bip_mov_ik` usage's loop displacement.
4. **Tracks are stored in FBX node order, and the engine reads slot i as bone i of the skeleton's
   list.** Blender writes nodes depth-first. `human_skeleton`'s list is depth-first; `horse_skeleton`'s
   is not (neck last), so a horse clip from the true hierarchy plays the tail on the neck. Export from a
   hierarchy whose depth-first walk equals the list (`horseneck1` under `horsetail3`, as TaleWorlds'
   own horse and goat FBX have it), with each local relative to its **engine** parent.
5. **The take name is the master's identity.** The master is named after the FBX take (the Blender
   action). A re-import under the same take keeps its GUID and its clips. A `.001` suffix makes a new
   master with an empty skeleton, and the old clip loses its animation.

TpacTool's `FixBoneForBlender` export has bone frames 90 to 180 degrees off the engine's: use its
meshes, not its bones. Optional helpers, where Blender and TpacTool do the same by hand: TAOM's
[transfer_clip_to_engine_rig.py](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/transfer_clip_to_engine_rig.py)
moves a clip onto an engine-frame rig with points 2 to 4 handled;
[read_anim_keyframes_tpac.ps1](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/read_anim_keyframes_tpac.ps1)
reads a master back (set `-TpacToolBin`, `-NativeDir` and `-OutDir`; the defaults are one machine's).

## Next

[The XML that binds it all together](/guides/custom_creature_xml/).
