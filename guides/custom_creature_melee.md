# Custom Creature: melee attack clips

Why a race's own swing clip can crash the game on its first swing, how the engine picks a swing clip
by weapon balance, and how to make a new swing safe.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. Read
[the clip inspector](/guides/custom_creature_clip_inspector/) first: this page sets several of its
fields.

!!! note "Version"
    Measured against **Bannerlord v1.5.3**. The engine-code findings come from TAOM's reverse
    engineering of the v1.5.3 game and Modding Kit DLLs; native crash offsets move with every engine
    update.

## Two kinds of attack

A creature mount attacks through code: your module plays an action and applies the damage itself,
outside the engine's melee system
([the battle page](/guides/custom_creature_battle/#scripted-creature-attacks)).
Vanilla horses have no attack clip besides the kick
([the reskin trap](/guides/custom_creature_xml/#the-reskin-trap)).

A humanoid race fights like a human soldier, driven by the engine's own melee system: a wind-up
(`act_ready_*`), a release, a blocked recoil when the blow is blocked, and weapon collision during
part of the release. **This page is about the second kind.**

The **Modding Kit** is TaleWorlds' editor that imports FBX files and writes `.tpac` asset packages.
An **action** is a named engine action such as `act_release_overswing_2h`, declared in
`action_types.xml` as `<action name="..." type="...">`; its **action type** is the kind that `type`
names (`actt_release_melee` for that one). An **action set** in `action_sets.xml` binds each action
to a clip for one skeleton; there the attribute holding the action's name is also called `type`. A
**clip** (AnimationClip) is a metadata record that cuts frames from a keyframe
**master** (SkeletalAnimation); see
[a clip is a record](/guides/custom_creature_clip_inspector/#a-clip-is-a-record-not-keyframes).

## The melee attack table

When a package loads, the engine files some clips into a **melee attack table**, keyed by clip
index, with ten clip slots per row. When an agent swings, the engine finds the row of the clip bound
to the swing action, clamps the weapon's balance to 0.05 to 0.95, and plays slot
`trunc(balance x 10)`, 0 to 9. An agent swinging with no weapon reads a balance of 1.0, which the
clamp turns into 0.95: slot 9. So two weapons can play different clips for one action.

In vanilla every clip with a row is a release or blocked clip, and the 296 `as_human_warrior`
actions bound to such clips all belong to four families: `act_release_*`, `act_quick_release_*`,
`act_blocked_*` and `act_quick_blocked_*`. **A clip you bind to one of these four families needs a
row.** Everything else (wind-ups, defends, parries, kicks, bashes, hit reactions, falls) binds like
any other clip.

Vanilla's 6,177 clips include 175 self-keyed, 107 twin-keyed and 1,070 generated children,
explained next.

!!! note "Not every release reaches the table"
    Vanilla binds 280 `_balanced` actions and 42 ranged releases to clips with no row, and humans do
    not crash. TAOM's reading is that those actions never reach the table; which paths use it is not
    fully mapped, so do not rely on the exception.

## The key is the Blends with animation box

A clip gets a row one way only: the **Blends with animation** box in the Kit's clip inspector. The
engine reads it once, at load. No flag sets it, and the model viewer never builds the table, so a
clip with the box empty looks perfect in the Kit.

| Blends with animation holds | Result at load |
|---|---|
| the clip's own name | **self-keyed**: a row keyed by this clip, all ten slots pointing at it |
| the name of its balanced twin | on Save the Kit generates ten children between the two, and they fill the row |
| nothing | **no row** |

Self-keying is what vanilla does for swings with no twin: fists, lances, staff thrusts. All 175
leave **Blends with action** empty; copy that. The name must fit in 63 characters
([clip names](/guides/custom_creature_clip_inspector/#clip-names)).

Twin-keying is the vanilla pattern for weapon swings: `release_overswing_2h` names
`release_overswing_2h_balanced`. On Save the Kit generates ten blend children, recording both
parents and a child index 0 to 9 in hidden fields; each fills the slot of its index in the first
parent's row. A missing twin logs `<name> not found for combat animation blending!` in the Kit.

**Blends with action** is a different box: it names an *action* whose clip plays together with this
one at runtime (an unknown name becomes -1, silently). Blends with animation names a *clip*, never
reaches the runtime clip, and is `UnknownClipName` in [TpacTool](/resources/tpactool/).

!!! danger "Never name a vanilla clip in the box, and never give your clip a vanilla parent"
    A generated child fills a slot in its parent's row, so a vanilla parent rewrites part of a
    vanilla row and changes that attack for every human in the game. The parent fields have no
    inspector control. A clip whose parent names are vanilla clips joins a vanilla row: do not ship
    it.

!!! warning "Check the box on every clip made from an override"
    The site's [Animations page](/modding/animations/) makes a new clip by overriding an existing
    one and renaming it (a rename in the Kit has its own trap:
    [clip names](/guides/custom_creature_clip_inspector/#clip-names)). Whether the override copies
    the original's Blends with animation or its parent fields is not established, so treat such a
    clip as a clone: set the box yourself (its own name on the four families, empty elsewhere). The
    parent fields do not show in the inspector; the tool under [procedure B](#procedure-b-self-key-your-own-clip) refuses a clip that has them.

## The first-swing crash

**Signature.** A crash to desktop on the new race's first swing, or the first time one of its
blows is blocked. On v1.5.3 it is an access violation at
`TaleWorlds.Native.dll+0x6590B9` reading an address from 0x8 to 0x50; the offset moves with every
engine update.

**Cause.** A release or blocked action whose clip has no row, or one the race's set does not bind
at all (the log then has a `does not contain` line). The lookup misses and the engine reads a clip
slot from a null row. TAOM's hill troll, a 3.6 m race on its own skeleton, hit it with clips
**cloned from vanilla templates**: copies of vanilla clips' metadata, made by a script that left
the box blank. A clip made by any tool that copies clip metadata counts, and so, possibly, does a
Kit override. **A clip cloned from a vanilla template must be keyed, or it has no row.**

**Why it survives testing.** Every clip looks right in the Kit. Making only the releases safe is not
enough either: on the hill troll the next crash was a blocked recoil.

**How it was found.** An action trace showed trolls winding up and never releasing, with no enemy
within 8 m: the swing crashed, not the hit. The dumps held the missed hash key, a clip index, in a
register; logging every action whose `MBActionSet.GetAnimationIndexOfAction` equalled it named the
clip ([the method](/guides/custom_creature_troubleshooting/#debugging-a-native-crash)).

**Fix.** One of the three ways below.

## Three ways to make a swing safe

| Way | How | Status |
|---|---|---|
| **Bind the vanilla clip** ([procedure A](#procedure-a-bind-the-vanilla-clip)) | the four families bind vanilla human clips; every other action keeps the race's clip | **proven in a battle**: no crash, but the race swings with human motion |
| **Self-key the race clip** ([procedure B](#procedure-b-self-key-your-own-clip)) | its own name in Blends with animation, Blends with action empty | all ten balance slots point at the one clip; on the quick families, **bound in battle with no crash, motion not yet confirmed**; the full families not yet tested |
| **Author a balanced twin** | a second clip for the balanced end, named in the box | the vanilla pattern; not tried by TAOM |

A race that inherits `as_human_warrior` and rebinds none of the four families is already on the
first way: TAOM's cave troll, a race scaled on the human skeleton, fought on inherited swings.

Start with the first way to get a race that fights at all. Human clips look wrong on a rest pose
unlike the human's ([why](/guides/custom_creature_race/#clips-re-framing-fixes-axes-not-poses)), so
then self-key the race's own swings, one family at a time, testing each in a battle.

## The procedure

Only the four families need either procedure, and one set can mix both. TAOM's hill troll set,
trimmed to four actions:

```xml
<action_set id="as_hill_troll_warrior" skeleton="troll_skeleton_a" movement_system="bipedal">
	<!-- trimmed: four of the set's actions, in file order -->
	<action type="act_ready_overswing_2h" animation="anim_hill_troll_ready_overswing_2h" />
	<action type="act_quick_release_overswing_2h" animation="anim_hill_troll_quick_release_overswing_2h" />
	<action type="act_quick_release_overswing_2h_balanced" animation="quick_release_overswing_2h_balanced" />
	<action type="act_release_thrust_1h" animation="release_thrust_1h" />
</action_set>
```

The wind-up binds the troll's own clip, the quick overswing its self-keyed clip (procedure B), the
one-handed thrust the vanilla clip (procedure A). TAOM leaves the `_balanced` actions on their
rowless vanilla clips, as vanilla does.

### Procedure A: bind the vanilla clip

1. **Bind every action of the four families to the clip `as_human_warrior` binds there.** A set
   with `base_set="as_human_warrior"` already has them unless it rebinds one. A standalone set (its
   own `skeleton=`, no `base_set`) must list every such action, since an unbound one crashes like a
   missing row.
2. **Bind the race's own clips** to every other action.
3. **Fight a Custom Battle** and read the log, as in steps 9 and 10 of procedure B.

### Procedure B: self-key your own clip

!!! note "Bound in battle for the quick families only, motion not yet confirmed"
    TAOM's hill troll binds its own self-keyed clips to the twelve two-handed quick release and
    quick blocked actions, `act_quick_release_*` and `act_quick_blocked_*` for overswing, slash left
    and slash right, in both stances. Two v1.5.3 Custom Battles on those bindings ran with no crash,
    and TAOM's clip trace, logging in the second, showed the troll entering all twelve. The trace
    names the clip bound to each action, not the frames drawn, so the motion has not been
    confirmed by eye; all twelve clips sit at Loading Type 2. The full-length `act_release_*` and
    `act_blocked_*` actions have not come up in a test.

1. **Key every race clip bound to the four families.** The clip's own name in Blends with
   animation, Blends with action empty.
2. **Set Loading Type to 0, Always keep in memory.** It is the conservative choice, and all 175 of
   vanilla's self-keyed clips sit at 0. It is not a proven requirement: vanilla has 347 unkeyed
   clips at 2, Never load, and Native's `action_sets.xml` binds 254 of them directly to actions
   that visibly play (conversation gestures, tavern drinking, the cat and dog gaits), so 2 does not
   by itself stop a directly bound clip from playing. What plays through a self-keyed row at 2 is
   not established. A clip cloned from a template keeps the template's value, and many vanilla
   swings sit at 2 ([the fields](/guides/custom_creature_clip_inspector/#the-fields-one-by-one)).
3. **Give it a combat parameter id with a collision window** whose `hit_bone_index` lands on your
   skeleton's weapon hand ([next section](#combat-parameters-the-hit-window-lives-outside-the-clip)).
4. **Run Compute Reach** with your race's skeleton model in its Skeleton combo, not the default
   `human_skeleton` (TAOM's reading: it takes bone indices from the model named there). It writes
   the longest thorax to main-hand distance over 30 samples into Param 1: on a release clip, the
   reach in metres (vanilla 0.58 to 1.2), which the AI appears to read.
5. **Copy the nearest vanilla clip's priority and flags.** The Kit's default priority, 0, can play
   in the viewer and lose in battle
   ([why](/guides/custom_creature_clip_inspector/#priority-why-a-clip-plays-in-the-viewer-and-not-in-battle)).
6. **Keep both hand poses from 0 to 4**; the game does no upper bound check
   ([hand morph channels](/guides/custom_creature_race/#face-and-hand-morph-channels)).
7. **Save in the Kit and check the RuntimeDataCache entry.** Save writes the clip and cooks its
   `.rdc` cache entry, without which the game skips the package silently
   ([what Save writes](/guides/custom_creature_clip_inspector/#what-save-writes-and-the-runtimedatacache)).
8. **Bind** the clips in the race's action set.
9. **Fight a Custom Battle** with a light weapon, then a heavy one, so slot 0 and slot 9 both come
   up. Watch the first swings and blocked blows.
10. **Read the log**, `C:\ProgramData\Mount and Blade II Bannerlord\logs\rgl_log_<pid>.txt`, for
    `Combat parameter not found:`, `Please set hit_bone_index` and `does not contain`
    ([log strings](/guides/custom_creature_troubleshooting/#what-to-search-for-in-the-log)).

TAOM's optional
[`set_clip_balance_name.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/set_clip_balance_name.py)
does step 1 offline (`--apply`, then `--check`), rewrites each item checksum and RuntimeDataCache
stamp to match, and refuses to run while the Kit or the game does; the inspector does the same by
hand. Even a dry run refuses a clip whose parent fields are set and names them. Pass `--clips-dir`
and `--module`, since the defaults are TAOM's. It needs `xxhash` and a helper from the same
[tools folder](https://github.com/haterade22/TAOM/tree/bannerlord-1.5.x/tools).

## Combat parameters: the hit window lives outside the clip

The clip's combat parameter id names an entry in `Native/ModuleData/combat_parameters.xml` saying
when in the clip weapon collision runs, which bone carries the weapon, and how far the attacker may
turn and look. The v1.5.3 file has 137 live entries (165 in its text, 28 commented out). Its
`1h_up` entry, copied from that file:

```xml
<combat_parameter
	id="1h_up"
	collision_check_starting_percent="0.38"
	collision_check_ending_percent="0.50"
	vertical_rot_limit_multiplier_up="0.3"
	vertical_rot_limit_multiplier_down="0.3"
	left_rider_rot_limit="overswing_rider_rot_limit_left"
	left_rider_min_rot_limit="up_min_rider_rot_limit_left"
	right_rider_rot_limit="overswing_rider_rot_limit_right"
	right_rider_min_rot_limit="up_min_rider_rot_limit_right"
	rider_look_down_limit="overswing_rider_look_down_limit"
	left_ladder_rot_limit="overswing_ladder_rot_limit_left"
	right_ladder_rot_limit="overswing_ladder_rot_limit_right"
	hit_bone_index="27"
	shoulder_hit_bone_index="21" />
```

* `collision_check_starting_percent` and `_ending_percent` bound, as fractions of clip progress,
  when weapon collision runs. The file's header comment gives defaults of 1.0 and 0.0: no window.
  **A retimed clip keeps its old window**, so pick an id whose window matches where your weapon
  lands.
* `hit_bone_index` is a raw bone index into the agent's skeleton. Counted from 0 down
  `human_skeleton`'s bone list, vanilla's values land where you would expect: most entries use 27,
  `r_finger0` (the right-hand grip bone), the left fists 20, `l_finger0`, and the kicks 7 and 3,
  `r_foot` and `l_foot`. A race that keeps the human's bone names and list order
  ([the race skeleton](/guides/custom_creature_race/#the-skeleton-human-names-human-order-human-axes))
  keeps those indices, as TAOM's hill troll does; the full human list is in the table below. On
  another skeleton, count to the weapon bone in its own list. TpacTool shows vanilla's skeletons
  with their bones
  ([getting the real skeleton](/guides/custom_creature_skeleton/#getting-the-real-skeleton)), though
  `human_skeleton` is not in `skeletons.tpac` but in Native's `EmAssetPackages/human/human.tpac`
  ([fingerprints](/guides/custom_creature_reference/#known-good-skeleton-fingerprints)), which
  TpacTool's library reads. Whether it opens a race's own package is not established. A missing or
  negative hit bone logs `Please set hit_bone_index` (the file's comment says a negative value lets
  the combat system choose).
* An id that does not resolve logs `Combat parameter not found:` and leaves the swing with no
  window (read from the engine code, not reproduced in game).

??? note "Bone indices of `human_skeleton`"
    Read from Native's `human_skeleton` on v1.5.3 with TpacTool's library: 28 bones. These are
    `human_skeleton`'s indices; a race on its own skeleton shares them only if it keeps this order.

    | Index | Bone | Index | Bone |
    |---|---|---|---|
    | 0 | `pelvis` | 14 | `l_clavicle` |
    | 1 | `l_thigh` | 15 | `l_upperarm_twist` |
    | 2 | `l_calf` | 16 | `l_upperarm_twist1` |
    | 3 | `l_foot` | 17 | `l_foretwist` |
    | 4 | `l_toe0` | 18 | `l_foretwist1` |
    | 5 | `r_thigh` | 19 | `l_hand` |
    | 6 | `r_calf` | 20 | `l_finger0` |
    | 7 | `r_foot` | 21 | `r_clavicle` |
    | 8 | `r_toe0` | 22 | `r_upperarm_twist` |
    | 9 | `spine` | 23 | `r_upperarm_twist1` |
    | 10 | `spine1` | 24 | `r_foretwist` |
    | 11 | `spine2` | 25 | `r_foretwist1` |
    | 12 | `neck` | 26 | `r_hand` |
    | 13 | `head` | 27 | `r_finger0` |

**A module's own entries are untested.** Native registers its file with this row in its
`ModuleData/project.mbproj`:

```xml
<file id="soln_combat_system" name="ModuleData/combat_parameters.xml" type="animation_combat_parameters" />
```

A module's `project.mbproj` can carry the same row, but whether the game merges that file with
Native's, and what an id both define does, is not established: no other TaleWorlds module ships
one, and TAOM has not tried it. Until someone does, **reuse the vanilla id whose window and hit
bone fit your swing.**

A scripted attack that applies damage in code is not bound by any window. TAOM's reverse
engineering found no native reader for `collision_radius`, `shoulder_hit_bone_index`,
`alternative_attack_cooldown_period` or the bashes' `<custom_collision_capsule>`: treat their effect
as unverified.

## What is still open

On the quick families, self-keyed clips are bound in battle with no crash, motion not yet
confirmed ([procedure B](#procedure-b-self-key-your-own-clip)); the rest is open.

!!! note "Not verified in game"
    * That a self-keyed clip's own motion is what the swing shows.
    * A self-keyed clip on the full-length `act_release_*` and `act_blocked_*` actions.
    * What plays through a self-keyed row whose clip is at Loading Type 2.
    * Which native callers use the table, and whether vanilla's twin-keyed swings always do.
    * That the balance behind the slot pick is the weapon's `WeaponBalance` (TAOM's reading).
    * Whether Compute Reach's item point includes a weapon tip offset.
    * Whether a Kit override copies Blends with animation and the parent fields.
    * Whether a module can add its own combat parameters.

## Next

[A humanoid race on its own skeleton](/guides/custom_creature_race/), and
[big creatures in battle](/guides/custom_creature_battle/) for scripted attacks.
