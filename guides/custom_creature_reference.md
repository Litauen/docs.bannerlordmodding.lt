# Custom Creature: reference tables

Lookup tables for creature work: animation clip flags, action types, the `.tpac` container format,
and known-good skeleton fingerprints.

Part of the [Custom Creatures](/guides/custom_creatures/) guide.

!!! note "Version"
    Flag values and their effects are checked against **Bannerlord v1.5.3**, the effects through TAOM's
    reverse engineering of its game and Modding Kit DLLs. The rest was measured on v1.4.8 to v1.5.3.

## Animation flags

### How the word is built

`AnimFlags` is a 64-bit word with two different things packed into it:

* **The low byte (`0xFF`) is a priority integer, not a set of bits.**
* **Everything above it is independent behaviour flags.**

Reading the low byte as bits is a real mistake with real consequences, because ORing a stray value
into it changes which clip wins an arbitration.

!!! warning "Code can add flags to one request; it cannot clear a clip's own"
    The Kit saves flags as a list of **names**: an unknown name is dropped silently, and a bit with no
    name cannot be saved. TAOM's reverse engineering of v1.5.3 found nine flags tested in seven managed
    files. `SetActionChannel` can OR extra flags into one request and replace
    its priority byte, but cannot clear a flag the clip was authored with. A row marked "no native
    consumer found" describes an unverified effect.

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
| `anf_synch_with_movement` | `0x2000000` | Takes the clip's progress from the agent's movement phase. On 65 vanilla rider and head-turn overlays, **none** of 435 human gait clips, and Artem's working elephant walks: allowed on a walk, not required. |
| `anf_displace_position` | `0x400000000000` | Moves the agent by the **displacement** clip usage's vector. **Needs that usage,** or the engine reads null. Deaths and other one-shots, not locomotion. |
| `anf_use_last_step_point_as_data` | `0x800` | Silences the fourth step point. On 62 vanilla equip clips; what reads the point as data is not established. |
| `anf_affected_by_movement` | `0x40000000000` | Clip is blended by movement state. Broader than synch. **No native consumer found on v1.5.3.** |
| `anf_lock_movement` | `0x1000000` | Pins the agent in place for the clip's duration. |
| `anf_enforce_root_rotation` | `0x8000000000` | Facing follows the clip's baked root rotation. Turn clips. |
| `anf_align_with_ground` | `0x100000000000` | On a human, ramps ground alignment over the **blend** clip usage's range. **Needs that usage,** or the engine reads null. |
| `anf_ignore_slope` | `0x200000000000` | The inverse: keep the authored orientation. **No native consumer found on v1.5.3.** |
| `anf_ignore_scale_on_root_position` | `0x1000000000000` | Apply root displacement without the body-scale multiply. Useful on a scaled creature whose lunge overshoots. **No native consumer found on v1.5.3.** |
| `anf_synch_with_horse` | `0x200000` | Rider clip synced to the mount's gait. Rider clips only. |

### Body enforcement and lifecycle

| Flag | Bit | What it does |
|---|---|---|
| `anf_cyclic` | `0x4000000000` | **Loops** the action at clip end. Not required on locomotion: 0 of 435 vanilla human gait clips and 48 of 142 `quad_movement` clips have it. A clip reused for an inventory or conversation idle takes that action's vanilla flags; without `cyclic` it plays once, by TAOM's reading. |
| `anf_enforce_all` | `0x2000000000` | Overrides the whole skeleton, no blend smear. Death, hard transitions. |
| `anf_enforce_lowerbody` | `0x1000000000` | Overrides the leg bones only. |
| `anf_allow_head_movement` | `0x10000000000` | Carves the head out so look-at keeps steering it. |
| `anf_keep` | `0x4000` | Freeze on the last frame. Do not combine with `anf_cyclic`. |
| `anf_restart` | `0x8000` | Re-trigger from frame 0 even if already playing. |
| `anf_disable_alternative_randomization` | `0x80000000` | **Not a clip flag:** no checkbox or saved name exists. Code passes it to `SetActionChannel` to skip the random pick among alternatives. |
| `anf_disable_auto_increment_progress` | `0x100000000` | The engine stops advancing progress. Never on a normal clip; it would freeze. |
| `anf_animation_layer_flags_mask` | `0xFFFF000000000` | Bits 36 to 51: ordinary Kit checkboxes, also passed to the renderer as layer bits. |
| `anf_animation_layer_flags_bits` | `0x24` | **The shift value (36) locating that field. This is metadata, not a flag.** Never OR it into a clip: it overlaps the priority byte. |

### IK, collision, physics

| Flag | Bit | What it does |
|---|---|---|
| `anf_disable_foot_ik` | `0x20000000000` | Turns off foot grounding. **Do not set on a grounded walk or run.** Do set on jump, rear and death. |
| `anf_disable_hand_ik` | `0x40000` | Hands play as authored. Sensible on creature clips, which have no grip target. **No native consumer found on v1.5.3.** |
| `anf_update_bounding_volume` | `0x80000000000` | Recompute cull and hit bounds from the live pose. **Wide-pose clips** (rear, lunge, death) so limbs sweeping past the rest bounds are not culled or mis-hit-tested. |
| `anf_disable_agent_agent_collisions` | `0x100` | Pass through other agents. Useful so a large creature's death does not bulldoze troops. **No native consumer found on v1.5.3.** |
| `anf_ignore_static_body_collisions` | `0x400` | Ignore world geometry, stay solid against agents. **No native consumer found on v1.5.3.** |
| `anf_ignore_all_collisions` | `0x200` | Ignore both. Dangerous; can sink through the floor. |

!!! note "Foot IK belongs to the human animation system"
    `anf_disable_foot_ik` makes the engine's human animation system skip foot IK. Nothing is established
    about foot grounding on a quadruped, or about which monsters run that system.

### Sound and networking

| Flag | Bit | What it does |
|---|---|---|
| `anf_make_walk_sound` | `0x20000` | Footstep foley in gait cadence. Walk and run clips. |
| `anf_make_bodyfall_sound` | `0x1000` | The heavy body-impact thud. Death and collapse. |
| `anf_attach_sound_to_agent` | `0x400000000` | Spawned sound follows the moving agent. |
| `anf_spawn_particle` | `0x800000000` | Spawns the **particle** clip usage's particle at its bone. **Needs that usage,** or the engine reads null. |
| `anf_do_not_keep_track_of_sound` | `0x20000000` | Fire and forget. |
| `anf_client_prediction` | `0x2000` | Multiplayer prediction on any client. Irrelevant in single player. |

Item-handling flags (`anf_stick_item_to_left_hand`, `anf_use_left_hand_during_attack`, the rope-weapon
pair, and the rest) are humanoid-only. Leave them unset on a creature.

### Per-clip recipe

Read from vanilla's own clips on v1.5.3; copy the nearest and judge it in a battle.

| Vanilla clip | Priority | Flags | Clip usages | Blend in / out |
|---|---|---|---|---|
| `walk_forward_unarmed` | 0 | `make_walk_sound` | `bip_mov_ik` | 0.3 / 0 |
| `troop_stand_unarmed_1` (idle) | 1 | `allow_head_movement` | none | 0.5 / 0 |
| `strike_chest_front` (hit reaction) | 80 | `client_prediction`, `restart`, `enable_hand_blend_ik`, `enforce_root_rotation`, `update_bounding_volume` | none | 0.1 / 0.1 |
| `death_fall_front` | 95 | `make_bodyfall_sound`, `client_prediction`, `keep`, `disable_hand_ik`, `lock_movement`, `enforce_all`, `enforce_root_rotation`, `disable_foot_ik`, `update_bounding_volume`, `align_with_ground`, `displace_position`, `reset_camera_height` | `blend`, `displacement` | 0.3 / 0 |
| `horse_kick` | 34 | `enforce_lowerbody`, `enforce_all` | not recorded | 0.2 / 0.4 |

Every vanilla horse clip read carries `enforce_lowerbody`. Artem's elephant walks use
`synch_with_movement` and `cyclic` instead, and also work. Runtime effects:
[the clip inspector](/guides/custom_creature_clip_inspector/#flags-at-runtime).

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

The engine's own classification constants. Of these, only `Rear` drives the
[reskin trap](/guides/custom_creature_xml/#the-reskin-trap): `Agent.Mount` refuses a mount whose
channel-0 action is typed `Rear`.

| Constant | Value |
|---|---|
| `ActionCodeType.Kick` | 28 |
| `ActionCodeType.Rear` | 47 |
| `StrikeBegin` | 48 |
| `ActionCodeType.MountStrike` | 52 |
| `StrikeEnd` | 52 |

`Agent.IsInBeingStruckAction` reads types from `StrikeBegin` up to but not including `StrikeEnd` (48 to
51) as **being struck**, so `MountStrike` (52) is not included.

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

The item `checksum` is xxHash64, seed 0, over `metadata_size` (as an int64) followed by the metadata;
each segment carries xxHash64 of its **decompressed** payload. Recompute it after any metadata edit
(TAOM's [tpac_fix_item_checksums.py](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/tpac_fix_item_checksums.py)
does). TpacTool.Lib writes zero, and such a package loads only with a
[RuntimeDataCache entry](/guides/custom_creature_clip_inspector/#what-save-writes-and-the-runtimedatacache).

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
means the physics data was dropped, which a re-import that rebuilds the skeleton does silently. A humanoid race carries the human's 34.

| Skeleton | Bones | Usage | Ragdoll constraints |
|---|---|---|---|
| `human_skeleton` | 28 | `human` | 34 (15 `d6`, 19 `ik`) |
| `horse_skeleton` | 32 | `horse` | |
| `skeleton_warg` | 49 | `horse` | 48 |
| `elephant_skeleton` | 60 | `horse` | 59 |
| `chariot_skeleton` | 60 | | |
| `spider_skeleton` | 62 | **`horse`** | |
| `troll_skeleton_a` (a humanoid race) | 28 | `human` | 34 |

Healthy creatures in this sample carry between 48 and 61 constraints.

`human_skeleton` is not in `skeletons.tpac` with the others: it lives in
`Modules/Native/EmAssetPackages/human/human.tpac`.

## Sources and credits

Flag values and engine constants are read from Bannerlord v1.5.3. The container format derives from
[TpacTool](https://github.com/szszss/TpacTool) by szszss, MIT licensed.

Measurements came out of building creatures for **TAOM (Tales From the Age of Men)**, working with
assets by **Artem** (ADOD_Beasts) and **Byak0** (Alliance, Alliance.Wargs).
