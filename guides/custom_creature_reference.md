# Custom Creature: reference tables

Lookup tables for creature work: animation clip flags, action types, the `.tpac` container format,
and known-good skeleton fingerprints.

Part of the [Custom Creatures](/guides/custom_creatures/) guide.

!!! note "Version"
    Read out of **Bannerlord v1.4.8**. Flag values are from the engine enum.

## Animation flags

### How the word is built

`AnimFlags` is a 64-bit word with two different things packed into it:

* **The low byte (`0xFF`) is a priority integer, not a set of bits.**
* **Everything above it is independent behaviour flags.**

Reading the low byte as bits is a real mistake with real consequences, because ORing a stray value
into it changes which clip wins an arbitration.

!!! warning "You cannot toggle these from a Harmony patch"
    Only **two** flags are ever bit-tested in managed C#: `anf_synch_with_ladder_movement` and
    `anf_displace_position`. Every other flag is consumed entirely inside the native engine DLL.

    You can OR additional flags in through `SetActionChannel`'s `additionalFlags` parameter, but you
    cannot inspect or clear a clip's authored flags from managed code. Set them in the Modding Kit.

### Priority levels (the low byte)

Higher wins. A request whose priority is greater than or equal to the current action takes the body.

| Level | Value | | Level | Value |
|---|---|---|---|---|
| `continue` (locomotion, idle) | `0x1` | | `rear` | `0x4A` |
| `jump` / `ride` / `crouch` | `0x2` | | `upperbody_while_kick` | `0x4B` |
| `attack` | `0xA` | | `striked` (hit reaction) | `0x50` |
| `cancel` | `0xC` | | `fall_from_horse` / `jump_loop` | `0x51` |
| `defend` | `0xE` | | `jump_end` | `0x52` |
| `parry` / `throw` / `blocked` / `parried` | `0xF` | | `die` | `0x5F` |
| `kick` | `0x21` | | `mask` (the extraction mask) | `0xFF` |
| `reload` | `0x3C` | | | |
| `mount` | `0x40` | | | |
| `equip` | `0x46` | | | |

For a creature: locomotion at `continue`, attacks at `attack`, hurt reactions at `striked`, death at
`die`.

### Movement and root motion

The category that decides whether your creature slides.

| Flag | Bit | What it does |
|---|---|---|
| `anf_synch_with_movement` | `0x2000000` | Time-warps a locomotion clip to the agent's real ground speed. **The main anti-skate flag. Required on walk, run and turn.** |
| `anf_displace_position` | `0x400000000000` | **Root motion on:** the clip's baked root travel moves the agent. One-shot lunges only, **never** on a cyclic walk or run. |
| `anf_use_last_step_point_as_data` | `0x800` | Marks the stride-reference clip the gait builder samples for stride length. |
| `anf_affected_by_movement` | `0x40000000000` | Clip is blended by movement state. Broader than synch. |
| `anf_lock_movement` | `0x1000000` | Pins the agent in place for the clip's duration. |
| `anf_enforce_root_rotation` | `0x8000000000` | Facing follows the clip's baked root rotation. Turn clips. |
| `anf_align_with_ground` | `0x100000000000` | Tilts the body to the terrain normal. |
| `anf_ignore_slope` | `0x200000000000` | The inverse: keep the authored orientation. |
| `anf_ignore_scale_on_root_position` | `0x1000000000000` | Apply root displacement without the body-scale multiply. Useful on a scaled creature whose lunge overshoots. |
| `anf_synch_with_horse` | `0x200000` | Rider clip synced to the mount's gait. Rider clips only. |

### Body enforcement and lifecycle

| Flag | Bit | What it does |
|---|---|---|
| `anf_cyclic` | `0x4000000000` | **Loops.** Required on idle and all locomotion, or they play once and stop. |
| `anf_enforce_all` | `0x2000000000` | Overrides the whole skeleton, no blend smear. Death, hard transitions. |
| `anf_enforce_lowerbody` | `0x1000000000` | Overrides the leg bones only. |
| `anf_allow_head_movement` | `0x10000000000` | Carves the head out so look-at keeps steering it. |
| `anf_keep` | `0x4000` | Freeze on the last frame. Do not combine with `anf_cyclic`. |
| `anf_restart` | `0x8000` | Re-trigger from frame 0 even if already playing. |
| `anf_disable_alternative_randomization` | `0x80000000` | Opt this clip out of the random variant pool. |
| `anf_disable_auto_increment_progress` | `0x100000000` | The engine stops advancing progress. Never on a normal clip; it would freeze. |
| `anf_animation_layer_flags_mask` | `0xFFFF000000000` | A 16-bit layer-routing field at bits 36 to 51. Do not hand-set. |
| `anf_animation_layer_flags_bits` | `0x24` | **The shift value (36) locating that field. This is metadata, not a flag.** Never OR it into a clip: it overlaps the priority byte. |

### IK, collision, physics

| Flag | Bit | What it does |
|---|---|---|
| `anf_disable_foot_ik` | `0x20000000000` | Turns off foot grounding. **Do not set on a grounded walk or run.** Do set on jump, rear and death. |
| `anf_disable_hand_ik` | `0x40000` | Hands play as authored. Sensible on creature clips, which have no grip target. |
| `anf_update_bounding_volume` | `0x80000000000` | Recompute cull and hit bounds from the live pose. **Wide-pose clips** (rear, lunge, death) so limbs sweeping past the rest bounds are not culled or mis-hit-tested. |
| `anf_disable_agent_agent_collisions` | `0x100` | Pass through other agents. Useful so a large creature's death does not bulldoze troops. |
| `anf_ignore_static_body_collisions` | `0x400` | Ignore world geometry, stay solid against agents. |
| `anf_ignore_all_collisions` | `0x200` | Ignore both. Dangerous; can sink through the floor. |

!!! warning "The vanilla rig grounds only two feet"
    Foot IK solves for `r_foot` and `l_foot`. A creature with more than two legs can only ever have
    two of them grounded by the engine. Plan the gait around that rather than expecting eight-legged
    terrain adaptation.

### Sound and networking

| Flag | Bit | What it does |
|---|---|---|
| `anf_make_walk_sound` | `0x20000` | Footstep foley in gait cadence. Walk and run clips. |
| `anf_make_bodyfall_sound` | `0x1000` | The heavy body-impact thud. Death and collapse. |
| `anf_attach_sound_to_agent` | `0x400000000` | Spawned sound follows the moving agent. |
| `anf_spawn_particle` | `0x800000000` | Enables baked particle keys. Blender-authored clips have none. |
| `anf_do_not_keep_track_of_sound` | `0x20000000` | Fire and forget. |
| `anf_client_prediction` | `0x2000` | Multiplayer prediction on any client. Irrelevant in single player. |

Item-handling flags (`anf_stick_item_to_left_hand`, `anf_use_left_hand_during_attack`, the rope-weapon
pair, and the rest) are humanoid-only. Leave them unset on a creature.

### Per-clip recipe

Confirmed against shipped clips:

| Clip type | Flags |
|---|---|
| **Walk / movement** | `anf_synch_with_movement` + `anf_cyclic` |
| **Attack** | `anf_lock_movement` + `anf_enforce_all` |

Convention, and worth confirming in the Kit against a shipped creature rather than taking on faith:

| Clip type | Flags |
|---|---|
| **Run / canter / gallop** | the walk set + `anf_enforce_lowerbody` + `anf_make_walk_sound` |
| **Turn left / right** | `anf_enforce_root_rotation` + `anf_synch_with_movement` + `anf_cyclic` |
| **Idle / stand** | `anf_cyclic` + `anf_align_with_ground` |
| **Stand-for-movement-data** | `anf_use_last_step_point_as_data` |
| **Death** | `anf_enforce_all` + `die` priority, **no** `anf_cyclic` |
| **Rear** | `anf_enforce_all` + `rear` priority |

!!! note "Flags are not the same thing as clip usages"
    None of the above is `quad_movement`, which is a **clip usage** and lives in a separate section
    of the Kit's clip panel. See
    [the animation page](/guides/custom_creature_animation/#quad_movement-or-the-six-hour-crash).

## Action types

The engine dispatches on an action's **type**, so these are not cosmetic. For a creature mount
`<c>`, in `action_types.xml`:

| Action | Type |
|---|---|
| twelve falls: `act_<c>_fall_{right,left,roll,backwards,slow_right,slow_left}` and each `_continue` | `actt_fall` |
| `act_<c>_rear`, `act_<c>_rear_damaged` | `actt_rear` |
| `act_<c>_dash` | `actt_dash` |
| `act_<c>_kick` | `actt_kick` |
| `act_<c>_quick_stop`, `act_<c>_quick_stop_when_fast` | `actt_mount_quick_stop` |
| `act_<c>_hit_object`, `act_<c>_hit_object_while_falling` | `actt_hit_object` |
| `act_<c>_strike_front`, `act_<c>_strike_back` (heavy) | `actt_mount_strike` |
| `act_<c>_strike_front_while_moving`, `_back_while_moving` (light) | **untyped** |
| `act_<c>_idle_1` | `actt_idle` |
| `act_<c>_jump`, the one named by `jump_start_action` | **`actt_dash`, never `actt_jump`** |

The engine's own classification constants, which is what makes the
[reskin trap](/guides/custom_creature_xml/#the-reskin-trap) work the way it does:

| Constant | Value |
|---|---|
| `ActionCodeType.Kick` | 28 |
| `ActionCodeType.Rear` | 47 |
| `StrikeBegin` | 48 |
| `ActionCodeType.MountStrike` | 52 |
| `StrikeEnd` | 52 |

Anything in `StrikeBegin .. StrikeEnd` is read by `Agent.IsInBeingStruckAction` as **being struck**.

## The `.tpac` container format

Useful if you are writing tooling. Reverse-engineered from
[TpacTool](https://github.com/szszss/TpacTool) (MIT).

### Header, 36 bytes

```
0..3    magic 'TPAC'  (0x43415054)
4..7    version       (2 in all 6,107 files measured)
8..23   package guid
24..27  item count
28..35  TOC SIZE      <- the engine derives data_start = 36 + toc_size
```

!!! danger "Offset 28 is the field that eats tooling"
    A parser that skips those 8 bytes leads to a writer that zeroes them, and a zeroed TOC size
    makes the engine read the table of contents as vertex data. See
    [the assert](/guides/custom_creature_troubleshooting/#assert-about-rglvec3-while-loading-packages).

    Measured on 250 shipped tpacs across four modules: `header[28:36]` equals the sum of the item
    TOC lengths in **250 of 250**, and the first segment's data offset is always exactly
    `36 + tail`.

### Item

```
type_guid(16) | item_guid(16) | item_version(u32, if container version > 1)
| name(sized string) | metadata_size(i64) | metadata | checksum(i64)
| segment_count(i32) | segment_count x segment_header
| udep_count(i32) | udep_count x 48 bytes
```

Segment header: `offset u64, actual_size u64, storage_size u64, owner_guid 16, type_guid 16,
unknown u64, unknown u32, storage_format u8` where storage format 0 is raw and 1 is LZ4HC.

### Segment type GUIDs

| GUID | Type |
|---|---|
| `c635a3d5-eabb-45dd-883e-aa57e4196113` | Skeleton |
| `11d07d37-e720-406b-ab67-c846f96a8771` | SkeletonDefinitionData |
| `9b6ac06d-a546-40af-a555-40d301ab4b2f` | SkeletonUserData |
| `5fce4668-0596-c44b-8db2-1edaa9408411` | Geometry |

### Skeleton payloads

**SkeletonDefinitionData:** `name | bone_count(i32) | bone_count x { name, parent_index(i32, -1 for
root), rest_frame(16 floats) }`

**SkeletonUserData:** bounding box, then `usage` (`horse` / `human` / `other`), then a body list and
a constraint list. Each **Body** carries `bone_name`, `body_type` (`abdomen`, `none`, and so on),
`mass`, ragdoll and collision positions and radii. Each **Constraint** carries a type (`d6`, `hinge`,
`ik`), the child and parent bone, a rotation quaternion, a position, and for a `d6` six lock states
(`locked` / `limited` / `free`) plus their limits in radians.

!!! note "A skinned mesh stores bone INDICES, not bone names"
    Bone names live in the Skeleton item. A mesh that binds to a skeleton in a **different** tpac
    has no reason to carry a single bone name, and correctly does not.

    So scanning a mesh tpac for bone names is not a skinning test: it is a test for "does this file
    contain a Skeleton item", and every creature sharing an existing skeleton fails it. Test the FBX
    instead, before it reaches the Kit:

    ```
                     LimbNode  Deformer  Cluster  Skin  BindPose
    skinned export     98        1001      490     10      20
    unrigged source     0           0        0      0       0
    ```

## Known-good skeleton fingerprints

For sanity-checking a compile. A healthy creature skeleton has a real `usage`, typed collision
bodies, and roughly one ragdoll constraint per bone. Zero constraints with every body typed `none`
means the physics data was dropped, which an FBX re-import does silently.

| Skeleton | Bones | Usage | Ragdoll constraints |
|---|---|---|---|
| `human_skeleton` | 28 | `human` | |
| `horse_skeleton` | 32 | `horse` | |
| `skeleton_warg` | 49 | `horse` | 48 |
| `elephant_skeleton` | 60 | `horse` | 59 |
| `chariot_skeleton` | 60 | | |
| `spider_skeleton` | 62 | `other` | |

Healthy creatures in this sample carry between 48 and 61 constraints.

`human_skeleton` is not in `skeletons.tpac` with the others: it lives in
`Modules/Native/EmAssetPackages/human/human.tpac`.

## Sources and credits

Flag values and engine constants are read from the shipped assemblies and asset packages of
Bannerlord v1.4.8. The container format derives from
[TpacTool](https://github.com/szszss/TpacTool) by szszss, MIT licensed.

Measurements came out of building creatures for **TAOM (Tales From the Age of Men)**, working with
assets by **Artem** (ADOD_Beasts) and **Byak0** (Alliance, Alliance.Wargs).
