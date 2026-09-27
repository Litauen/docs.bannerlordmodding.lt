# Custom Creature: the clip inspector

What each field of the Modding Kit's animation clip inspector does in game, which mistakes fail
silently, and what Save writes.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. Read
[the animation clips page](/guides/custom_creature_animation/) first, or for a race,
[the race page](/guides/custom_creature_race/).

!!! note "Version"
    Measured against **Bannerlord v1.5.3**. Engine-code findings come from TAOM's reverse engineering of the v1.5.3 game and Modding Kit DLLs; native crash offsets move with every engine update, and older measurements are marked where they appear.

## A clip is a record, not keyframes

The Modding Kit (the free editor, under Tools in Steam) writes each asset into a `.tpac`, the
engine's binary asset package of one or more items with GUIDs. An animation is two items:

* **The master** (SkeletalAnimation) holds the keyframes. The Kit makes one per FBX take, in
  `<take>_geo.tpac`.
* **The clip** (AnimationClip, `<clip>_anm.tpac`) holds no motion. It names its master by GUID and
  adds the frame range, duration, flags, priority, sounds, blend partners, a combat parameter and up
  to two clip usages.

!!! note "Usage means three things in this guide"
    A skeleton's **Usage** (`human`, `horse` or `other`), a Monster's **`monster_usage`** (naming its monster usage set), and a **clip usage**, a typed record on a clip ([below](/guides/custom_creature_clip_inspector/#clip-usages)). This page means the last.

**Action sets bind clips, never masters.** Vanilla keeps 6,177 clips in
`Native/AssetPackages/animation_clips.tpac` and 4,052 masters, under other names, in
`animations.tpac`. Several clips can cut one master, some backward, and a master named like a clip
can be an empty shell, so never match by name.

!!! note "`animation_clips.tpac` reads fine with its header only"
    [The skeleton page](/guides/custom_creature_skeleton/#getting-the-real-skeleton) warns that TpacTool throws on this package. TAOM recorded that throw in a script that loads a package's data at once (the TpacTool.Lib library's `loadDataNow` set to true). Opened header only, it lists every clip's master GUID, range, Duration and flags, which is how TAOM's [`read_anim_keyframes_tpac.ps1`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/read_anim_keyframes_tpac.ps1) `-ByClip` reads it (its defaults are one machine's paths: pass `-NativeDir`, `-TpacToolBin` and `-OutDir`).

```text
FBX take
   | Kit import
   v
<take>_geo.tpac      master: keyframes
   | New animation clip, Animation source = the master
   v
<clip>_anm.tpac      clip: metadata only
   | Save: checksum, RuntimeDataCache entry, blend children
   v
game launch          clip registered by name
   v
action_sets.xml      action bound to a clip name, per skeleton
   v
Monster, then Agent.SetActionChannel: priority decides what plays
```

An **action** is a named engine action such as `act_release_overswing_2h`, declared in
`action_types.xml` as `<action name="..." type="...">`; its **action type** is the kind in that
`type` attribute, such as `actt_kick`. An **action set** (`action_sets.xml`) binds actions to clips
for one skeleton (there the attribute naming the action is also called `type`), and the engine's
**action code** is the runtime index it gives an action name. A **monster usage set** tells the
engine which action to request, and when. Actions play on two **channels**: 0 for the whole body
and locomotion, 1 for the upper body ([SetActionChannel](/modding/animations/#setactionchannel)).

**The take name is the master's identity.** Re-import an FBX under the same take (the Blender
action's name) and the master keeps its GUID, so linked clips survive and only Source 2 follows a
new length. A `.001` suffix on the action makes a new master and orphans the clip, as happened once
to the head-butt of TAOM's war ram, a horse reskin.

After each import, check the master's Owner Skeleton: the Kit left it empty on all 149 masters from
two purchased Fab animation packs, with no warning, and their clips still played in the viewer.

## The fields, one by one

Every field meaning here comes from TAOM's reverse engineering of the v1.5.3 DLLs, not from
TaleWorlds documentation; **"TAOM's reading"** marks an inference from that code that nothing in
game has confirmed. The inspector edits a copy: **nothing reaches disk until Save**, and whether
closing it without saving discards the edits is not established. With several clips selected, a
differing field shows `----` and keeps each clip's own value unless you change it. Step points and
clip usages need a single selection.

**Range and timing**

| Field | What the game does | Trap |
|---|---|---|
| Animation source | Names the master | Setting it fills neither Duration nor Source |
| Duration | Playback length in seconds | With Loading Type 1 (Load when needed), Save warns if Duration is 3.0 s or less |
| Source 1, Source 2 | First and last master frame | Source 2 below Source 1 plays backward. Frame 0 is rest, so Source 1 = 1 ([feet above the ground](/modding/animations/#feet-above-the-ground)) |
| Sample Rate | Read-only: frame span over Duration | 36 frames over 1.2 s reads 30 |
| Blend in period | Cross-fade into the clip | Used when code passes -0.2, `SetActionChannel`'s default `blendInPeriod` |
| Blend out period | Cross-fade out | Continue to action fires as it starts |

The site's [animation notes](/modding/animations/#blendinperiod) call the -0.2 default too fast.
Passing -0.2 hands the fade to the clip's own Blend in period, so a fade that looks too fast means
that clip's period is short: raise it in the clip, or pass a positive value from code.

**Arbitration**

| Field | What the game does | Trap |
|---|---|---|
| Priority | 0 to 100; who owns a channel ([below](/guides/custom_creature_clip_inspector/#priority-why-a-clip-plays-in-the-viewer-and-not-in-battle)) | A new clip starts at 0 |
| Randomization weight | 0 to 15; the clip's weight in its `alternative_group` | Vanilla gives every grouped clip 1 or more |

`alternative_group` is an `<action>` attribute in `action_sets.xml`; Native's three
`act_sit_down_on_floor_*` actions share one. Requesting any of them makes the engine pick among the
group's clips at random, weighted by this field (a group whose weights sum to 0 plays the action
asked for).

**Events**

| Field | What the game does | Trap |
|---|---|---|
| Step points | Four progress points (0 to 1) that fire events | -1 leaves a point unused; they fire more than footsteps |
| Sound code | A sound event | A miss logs "Sound not found" and stores -1 |
| Voice code | A `<voice_type>` from `voice_definitions.xml` | Unknown becomes -1 |
| Facial animation id | A `face_animation_record` from `voices.xml` | A miss is logged, -1; human animation system only |

What a step point fires:

* point 0 or 1: the Sound code;
* a point with no sound, or point 2 or 3: the Voice code;
* any point, with `make_bodyfall_sound`: the bodyfall sound;
* any point, with `make_walk_sound`: footsteps;
* point 3, with `use_last_step_point_as_data`: nothing.

The **human animation system** (`Human_anim_system`) is one of the engine's three, beside a general
one and a horse one. Only it applies facial animation and hand poses, and it is where
`disable_foot_ik` turns foot IK off. Which monsters run it is not established. Clear human voices,
foley and facial ids from clips cloned from vanilla.

**Links and combat**

| Field | What the game does | Trap |
|---|---|---|
| Blends with action | An action whose clip plays alongside at a blend factor | Unknown name: -1, silently |
| Blends with animation | A clip name: the melee attack table key, read at load ([melee page](/guides/custom_creature_melee/#the-key-is-the-blends-with-animation-box)) | Empty on a swing clip: first-swing crash. TpacTool calls it `UnknownClipName` |
| Continue to action | The next action, started when blend out begins | Unknown: -1; one the set lacks: a likely crash |
| Left, Right hand pose | 0 to 4; hand morph key 5 x left + right + 1 | Needs hand channels ([race page](/guides/custom_creature_race/#face-and-hand-morph-channels)); no upper bound check |
| Combat parameter id | An entry in `combat_parameters.xml`: hit window, hit bone ([melee page](/guides/custom_creature_melee/#combat-parameters-the-hit-window-lives-outside-the-clip)) | A miss is logged and leaves no collision window |
| Param 1, 2, 3 | Depends on the action type | Below |

**Params.** Param 1 is the reach in metres on a release clip (vanilla 0.58 to 1.2), written by
Compute Reach from the skeleton in its combo (for a race, its own skeleton: TAOM's reading), and the
level-look blend factor on look-slope clips. Param 2 is phase progress on reload clips; Param 3's
only reader found is death timing.

**Loading Type** is 0 Always keep in memory, 1 Load when needed or 2 Never load. TAOM's reverse
engineering reads 2 as "load no keyframes" at load (574 vanilla clips use it); a third-party
measurement on v1.4.6 read 2 as "play motion stored in the clip itself". A clip cloned from a
template keeps the template's value.

**Set 0 on any clip you bind to a
[melee action](/guides/custom_creature_melee/#the-melee-attack-table).** It is the conservative
choice, and all 175 of vanilla's self-keyed swings use it, but it is not a proven requirement.
Vanilla has 347 unkeyed clips at 2, and Native's `action_sets.xml` binds 254 of them straight to
actions that animate in vanilla play (conversation gestures, tavern drinking, the cat and dog
gaits), so by TAOM's reading 2 does not by itself stop a directly bound clip from playing. For any
other clip you bind, 0 is the safe default too: 5,040 of vanilla's 6,177 clips use it.

!!! note "Not verified in game"
    What plays through a self-keyed row at Loading Type 2 is not established. TAOM's hill troll binds its own self-keyed clips, all at 2, to its twelve two-handed quick release and quick blocked actions (overswing, slash left and slash right, in both stances): bound in battle with no crash, motion not yet confirmed. TAOM's trace names the clip bound to each action, not the frames drawn. Its full-length release and blocked actions have not come up in a test.

    TAOM's war elephant, built from Artem's ADOD_Beasts clips, has eight attack clips at 2 with no motion of their own, not yet checked in battle.

**Do not interpolate** and **Do not optimize** have no consumer found.

## Priority: why a clip plays in the viewer and not in battle

Every request to play an action passes a priority gate: the request's own priority if code gives
one, else the clip's. **A request below the priority of what is playing is rejected; an equal one
wins.** `ignorePriority`, or an empty channel, skips the check. The value is the low byte of the
clip's flag word ([full list](/guides/custom_creature_reference/#priority-levels-the-low-byte)).

**Priority matters for a clip requested on top of something else,** such as an attack, a hit
reaction or a death, whether your code or the engine asks for it. The engine's bands: attack 10,
defend 14, parry, blocked and throw 15, kick 33, reload 60, mount 64, equip 70, `striked` (hit
reaction) 80, die 95. Walk and run clips are bound in movement tables, which the engine's movement
system drives (TAOM's reading). Vanilla keeps them and the idles at the bottom:
`walk_forward_unarmed` 0, the battle idle `troop_stand_unarmed_1` 1.

A new clip starts at priority 0 with no flags, and the model viewer plays it with nothing
competing. In battle it ties with a priority-0 walk, and a tie wins, so the gate alone does not
explain a clip that never shows. TAOM's reading is that locomotion takes channel 0 straight back;
how is not established: no source shows the movement system re-requesting its clip each tick.

!!! note "The war ram's head-butt is unresolved"
    The war ram's head-butt played in the Kit, and a Custom Battle logged 1,300 butts while the head never dropped (the code applied damage in the tick it requested the action, so the log proved nothing). At priority 34 the next battle logged 1,005, still invisible. The clip now carries vanilla `horse_kick`'s priority, flags and blends, not yet seen in battle.

Code can override per request: a nonzero low byte in `SetActionChannel`'s `additionalFlags` replaces
the clip's priority while the action plays, and the other bits are ORed onto its flags. **Code
cannot clear a flag the clip was saved with.**

!!! tip "Copy a vanilla recipe, then judge it in a battle"
    Take the priority, flags, blend periods and usages of the nearest vanilla clip of the same kind, then judge the clip in a battle, never in the viewer.

## Clip usages

A clip usage is a typed record in the collapsed Clip usages section below the flags. **At most two
survive at load**; a third is dropped with a log line. An unknown usage name (hand-built tpacs only)
misreads every later field.

| Usage | Needed by | If missing |
|---|---|---|
| `displacement`: vector, end progress | `displace_position` | null read (code) |
| `quad_movement`: loop displacement, pace limits | clips in horse-type movement tables | crash in every mount context (in game, v1.4.x) |
| `bip_mov_ik`: loop displacement, foot snapping | human locomotion tables | unchecked reads (code) |
| `blend`: start, end progress | `align_with_ground` on humans | null read (code) |
| `mount_change` | mount and dismount clips (likely) | depends on the reader |
| `hand_switch` | `switch_item_between_hands` | null read (code) |
| `particle` | `spawn_particle` | null read (code) |

Usages give in-place motion its travel back: vanilla `walk_forward_unarmed` carries 1.8 m per loop
in `bip_mov_ik`. Step points on a `quad_movement` clip add footsteps and were not shown to be needed
for safety (42 of 142 vanilla quadruped movement clips have none). Attack, hit and death clips carry
no `quad_movement` ([the crash](/guides/custom_creature_animation/#quad_movement-or-the-six-hour-crash)).

## Flags at runtime

The Kit's 43 flag checkboxes are saved as a list of names, and the game matches each against its own
table. An unknown name (only a hand-built tpac can hold one) is dropped silently, and a flag with no
name cannot be saved: `disable_alternative_randomization` works only as a request flag from code.

A load-time validator fixes these, logging each:

| Clip carries | The game |
|---|---|
| `keep` with `cyclic` or a Continue to action | drops `keep` |
| `cyclic` with a Continue to action | drops `cyclic` |
| `keep` or `cyclic` with a nonzero blend out | sets blend out to 0 |
| `blends_according_to_look_slope` without a Blends with action | drops the flag |
| `allow_head_movement` with `lock_camera` | drops `allow_head_movement` |

Flags with a traced effect:

| Flag | Effect |
|---|---|
| `keep` | Holds the last pose |
| `restart` | Re-requesting the playing action restarts it |
| `cyclic` | Replays the action at clip end, priority byte stripped |
| `enforce_all` | On channel 0: rejects channel 1 requests (unless `ignorePriority`), clears channel 1 |
| `enforce_lowerbody` | Picks the channel that owns footsteps; more not traced |
| `lock_movement` | Resets the agent's movement target at clip start |
| `make_walk_sound` | Footsteps at the step points |
| `make_bodyfall_sound` | Bodyfall sound at a step point |
| `do_not_keep_track_of_sound` | Sound plays untracked |
| `disable_foot_ik` | No foot IK in the human animation system |
| `synch_with_movement` | Progress from the movement phase |
| `synch_with_horse` | Progress from a value the engine passes each tick |
| `use_last_step_point_as_data` | Step point 3 stays silent |
| `blends_according_to_look_slope` | Blends with its partner by look pitch |
| `update_bounding_volume` | Sets the renderer's bound-update switch |
| `displace_position` | Moves the agent by the **displacement** usage |
| `align_with_ground` | Ground alignment over the **blend** usage (humans) |
| `spawn_particle` | The **particle** usage's effect |
| `switch_item_between_hands` | Weapon changes hands over the **hand_switch** usage |

**The last four need their usage**, which the code reads unchecked in places: without it, a null
read (not reproduced in game). No native consumer was found for `disable_agent_agent_collisions`,
`ignore_static_body_collisions`, `disable_hand_ik`, `stick_item_to_left_hand`,
`affected_by_movement`, `ignore_slope`, `ignore_scale_on_root_position` or the rope flags: harmless
to copy, not shown to do anything.

**`displace_position` does not read root motion from the keyframes**, though the animation page says
it turns root motion on. It moves the agent by the vector in the clip's displacement usage, linearly
up to the usage's end progress. All 337 vanilla clips with the flag carry that usage.

**Gait progress comes from the movement system, not flags** (TAOM's reading): `cyclic` is on 0 of
435 vanilla human gait clips and 48 of 142 quadruped ones, `synch_with_movement` on none of the
435. This corrects [the reference page's flag tables](/guides/custom_creature_reference/#animation-flags).

| Vanilla clip | Priority | Flags | Usages |
|---|---|---|---|
| `walk_forward_unarmed` | 0 | `make_walk_sound` | `bip_mov_ik` |
| `troop_stand_unarmed_1` (idle) | 1 | `allow_head_movement` | none |
| `strike_chest_front` (hit) | 80 | `restart`, `enforce_root_rotation`, `update_bounding_volume` and two more | none |
| `death_fall_front` | 95 | twelve, including `keep`, `enforce_all`, `align_with_ground`, `displace_position` | `blend`, `displacement` |
| `horse_kick` | 34 | `enforce_lowerbody`, `enforce_all`; blends 0.2, 0.4 | not recorded |

Every horse clip TAOM read carries `enforce_lowerbody`.

**A clip reused for another action must carry the flags of the vanilla clip bound to that action,**
not of the clip it resembles. In `as_human_warrior`, `act_inventory_idle` binds `inventory_idle`,
which is `cyclic`; the battle idle is not. TAOM's hill troll, a 3.6 m race on its own skeleton,
reused its non-cyclic idles for inventory, conversation and cheer actions, which those screens
request once, so by TAOM's reading of the engine code they play once and stop (not yet seen in
game).

## What Save writes, and the RuntimeDataCache

Save writes the metadata, computes the item checksum (xxHash64, seed 0, over the 8-byte metadata
length then the metadata), cooks the RuntimeDataCache entry and runs the blend-child generator for
twinned swings.

The **RuntimeDataCache** holds one cooked file per package,
`<module>/RuntimeDataCache/<package GUID>.rdc`. **The game renders a package only when its entry
exists, and only the Kit writes one, on a save.** Without it the package is skipped whole, with no
log line; masters are exempt and play anyway. TAOM's giant spider hit this: two skin variants cloned
by a tool drew nothing until the module was saved in the Kit.

!!! warning "A package written outside the Kit is invisible until the Kit saves it"
    Probe the loader before the bytes: redefine an existing item name in a throwaway package and search the log for `Overriding item`. No such line means the package never loaded.

The Kit scans packages only at startup and warns "External .rdc file modification detected" if an
entry changes under it: **write tpacs with the Kit closed**, then open the module and save.

A loose `Assets/` tree (one tpac per asset) and a cooked `AssetPackages/` tree can coexist; TAOM's
logs show the loose one loading (the loader's rule was not traced). Both claiming one name with a
reachable source crashes:
[the `rglIntrusive_ptr.h` assert](/guides/custom_creature_troubleshooting/#startup-assert-in-rglintrusive_ptrh151).

Tool writers: TpacTool.Lib writes a zero checksum, yet over a hundred such TAOM clips load because
they have entries; a mismatch between entry and checksum is untested. Round-trip a file unmodified
before rewriting it
([the rglVec3 assert](/guides/custom_creature_troubleshooting/#assert-about-rglvec3-while-loading-packages)).
Optional TAOM helpers:
[`check_rdc_entries.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/check_rdc_entries.py)
lists packages with no entry (its default module path is one machine's: pass `--module`);
[`tpac_fix_item_checksums.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/tpac_fix_item_checksums.py)
recomputes checksums. A Kit save does both.

## Clip names

A clip name is a fixed 64-byte string: **63 usable characters**. A longer one makes the Kit warn
`Could not set fixed-size(64) string`; pick a shorter name yourself, as TAOM did for 15 hill troll
names of up to 70 characters. Master names are not fixed-size. A duplicate clip name is refused
("Unable to register animation clip") and that clip is missing in game.

**The game knows a clip by the name stored inside its package, not by the file name.** TAOM's giant
spider keeps clips in files the Kit named `new_animation_clip_3_anm.tpac` and so on, whose items are
named `an_spi_idle` and the like. Renaming only the file does not rename the clip.

**No rename inside the Kit, with or without a restart, is known to give a usable clip.** As Artem
reported and TAOM's war ram repeated, a rename in the Kit corrupts the clip: the Kit keeps resolving
the old name, the inspector shows `Size in KB = 0`, Save is refused, the viewer draws a scrambled
pose, and the renamed file can vanish
([troubleshooting](/guides/custom_creature_troubleshooting/#a-clip-reports-zero-size-and-will-not-save)).
Artem's report says only a full restart of the Kit clears that state; nobody has recorded checking
the renamed clip after a restart, so treat the restart as recovery, not as a way to rename.

The site's [animation notes](/modding/animations/) create a clip as an override in the resource
browser and then rename it, which is this rename. What an override copies is not established; if it
copies the metadata, it brings Blends with animation (shown in the inspector) and a blend child's
parent fields (not shown): read
[the melee page](/guides/custom_creature_melee/#the-key-is-the-blends-with-animation-box) before
binding it to a swing.

The route TAOM uses never renames in the Kit:

1. Create the clip, leave it on the default `new_animation_clip` name, fill its fields and Save.
2. Close the Kit.
3. Rename the item inside the package with
   [`rename_anim_clip_tpac.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/rename_anim_clip_tpac.py)
   (a dry run unless you pass `--apply`). It keeps every GUID and the metadata and names the file
   `<name>_anm.tpac` to match. In a Kit-made clip package its whole edit is two fields, which a hex
   editor can make too: the 4-byte name length at byte 72 with the name after it, and the 4-byte
   size at byte 28, which must equal the file size minus 36.
4. Reopen the Kit. It loads a clip that was never renamed in a session.

## Next

[Melee attack clips](/guides/custom_creature_melee/), and for the log lines quoted here,
[what to search for in the log](/guides/custom_creature_troubleshooting/#what-to-search-for-in-the-log).
