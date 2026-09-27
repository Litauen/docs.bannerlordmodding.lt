# Custom Creature: a humanoid race on its own skeleton

How to give a two-legged race its own proportions: a skeleton that keeps the human's bone names and
axes, the human's physics carried across, the morph channels the engine expects, and clips that fit.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. The basic `skins.xml` race is on
[Races](/modding/races/), rigging to `human_skeleton` on [Armature/Skeleton](/3d/armature_skeleton/).
For a race on its own skeleton, read [the skeleton page](/guides/custom_creature_skeleton/) first.

!!! note "Version"
    Measured against **Bannerlord v1.5.3**. Engine-code findings come from TAOM's reverse engineering
    of the v1.5.3 game and Modding Kit DLLs; native crash offsets move with every engine update, and
    facts read on an older version are marked where they appear.

## Three ways to build a race

Decide this first; the cost rises steeply.

| Way | What it takes | Worked example | Proven in game |
|---|---|---|---|
| **On `human_skeleton`, scaled by the skin** | `min_scale` in the skin sets the size; `as_<race>_warrior` on `base_set="as_human_warrior"` overrides only your clips | TAOM's cave troll at `min_scale="1.9"`, its movement, hit and death clips retargeted from a purchased Fab animation pack | one battle: 2,982 blows taken, 19 deaths, no clip warning |
| **Own skeleton, the human's axes** | the human's names, order and axes at other lengths; the human's physics byte for byte; human clips as they are | TAOM's dwarf, its bones within about 35 degrees of the human's | TAOM's dwarf troops fight on it |
| **Own skeleton, other proportions and frames** | the rig re-framed to the human's axes, the human's physics carried through the rest pose, morph channels, its clips retargeted | TAOM's hill troll, 3.6 m tall on 28 bones | spawns and fights, hammer in hand; its own quick swings bound in battle with no crash, motion not yet confirmed; full-length swings untested |

**Armour follows the skeleton.** Armour rigged to `human_skeleton` fits the first row and floats on
the other two.

## What a race is, to the engine

A race is five pieces of data: a `<race>` in `skins.xml` (skeleton and meshes per gender and
maturity), a Monster (weight, heights, capsules, bone map, action sets), the action sets, a body
property (the face range a troop rolls), and `race="..."` on each troop. Several fail quietly
(loading code read on v1.4.8):

* **The race id is a number** from `skins.xml` merge order, human 0: append a new `<race>` at the
  end, since inserting one renumbers every race after it.
* **A misspelled `race=` does not fall back to human.** It throws `KeyNotFoundException`, an empty
  `catch` swallows it, and the rest of that character file is lost.
* **A character with no `<face>` renders as a toddler** reporting age 20. One malformed
  `<BodyProperty>` drops every entry after it.
* **`skins.xml` has no managed reader**, so copy a working race. Of a race's ten skins, only the two
  adult ones field soldiers.

The engine asks for five Monsters per race by suffix; a name that does not exist returns null,
silently:

| Monster id | Asked for by |
|---|---|
| `<race>` | every battle spawn |
| `<race>_child` | town and village children and teenagers |
| `<race>_settlement` | notables, shop workers, alley bosses, tavern hosts |
| `<race>_settlement_slow` | barbers, townsfolk, villagers |
| `<race>_settlement_fast` | the hurrying townsfolk |

!!! warning "A misspelled Monster variant fails only where it is asked for"
    Field-battle spawns ask only for `<race>`, so a bad variant survives every Custom Battle.
    TAOM's hill troll variants were named `troll_*` until the race was rewired; by TAOM's reading, a
    settlement character given null has no body. Read the ids back.

Action sets are named the same way (v1.5.3 code): `as_`, the race id, `_female` for a woman, then
one of 46 suffixes (`_warrior`, `_child`, `_poses`, `_facegen`, `_map` and more). The party,
inventory and encyclopedia previews and the map conversation ask for `as_<race>_warrior`; a missing
set asked for this way throws "Invalid action set code". **So a race needs its own family of sets,
even on `human_skeleton`**: one that only fights still shows in those previews, and how few sets a
race can ship with has not been tested.

### The cheapest race, step by step

Row 1 of the table: the site's [Races](/modding/races/) recipe plus what TAOM's cave troll needed.
The XML is the cave troll's, trimmed where marked.

**1. Register the files** in `ModuleData/project.mbproj` with the standard ids
([registration](/guides/custom_creature_xml/#registration-two-mechanisms-not-interchangeable)), as
Native registers its own `skins.xml`:

```xml
	<file id="soln_skins" name="ModuleData/skins.xml" type="skin" />
	<file id="soln_action_sets" name="ModuleData/action_sets.xml" type="action_set" />
	<file id="soln_monsters" name="ModuleData/monsters.xml" type="monster" />
```

Native and TAOM's Armory also list `monsters.xml` in `SubModule.xml`, as an
`<XmlName id="Monsters">` line in the form
[the XML page's registration section](/guides/custom_creature_xml/#registration-two-mechanisms-not-interchangeable)
shows.

**2. Add the `<race>`** to `skins.xml`: copy Native's `human` block, all ten skins, and change the
id, the meshes and `min_scale`, the size (1.9 here, 1.07 on the human):

```xml
	<race id="cave_troll">
		<skin
			gender="0"
			name="man"
			mesh_maturity_type="adult"
			morph_key="0"
			uses_stitching="true"
			body_mesh_suffix=""
			min_scale="1.9"
			skeleton="human_skeleton"
			body_meta_mesh="lotr_troll_body"
			body_meta_mesh_shoulders="body_male_a_sh"
			body_meta_mesh_upperbody="box_a"
			legs_mesh="lotr_troll_feet"
			hands_mesh="lotr_troll_hands"
			face_meta_mesh="lotr_troll_head"
			underwear_bottom_mesh=""
			underwear_top_mesh="">
			<!-- trimmed: deform_keys, voice_types, the hair, eyebrow, beard and tattoo lists, the
			     colour gradients, face_textures, mouth_textures and constraints, shaped as in
			     Native's human skin -->
		</skin>
		<!-- trimmed: nine more skins, the woman, then male and female teenager, tween, child and toddler -->
	</race>
```

**3. Add the Monster**, a copy of Native's `human`. **Keep its heights, `arm_length` and capsule**:
the skin supplies the size, and the v1.5.3 managed code multiplies `arm_length` by the agent's scale
for reach and the eye height for the follow camera. A mount's `body_length` scales its capsules
([size](/guides/custom_creature_battle/#size)); whether a race's skin scale does the same to the
Monster's capsule is not established. The cave troll kept the 0.37 capsule and fought at
`min_scale` 1.9.

The example is the cave troll's Monster as shipped. Against Native's `human` it changes weight, hit
points, both walking speeds, jump acceleration, two flags and the crouched capsule's lower end (0.6,
Native 0.8), and it omits Native's `female_action_set`, `standing_chest_height` and
`standing_pelvis_height`:

```xml
	<Monster id="cave_troll"
			   action_set="as_cave_troll_warrior"
			   monster_usage="human"
			   weight="160"
			   hit_points="200"
			   absorbed_damage_ratio="1.0"
			   walking_speed_limit="2.0"
			   crouch_walking_speed_limit="1.8"
			   jump_acceleration="5.0"
			   sound_and_collision_info_class="human"
			   standing_eye_height="1.70"
			   crouch_eye_height="1.10"
			   mounted_eye_height="0.75"
			   eye_offset_wrt_head="0.13, 0.1, 0.0"
			   first_person_camera_offset_wrt_head="0.136, 0.1, 0.0"
			   arm_length="0.9"
			   arm_weight="5.0">
		<!-- trimmed: 68 more attributes (jump_speed_limit, family_type, the bone map), every one
		     at Native's human value -->
		<Capsules>
			<body_capsule radius="0.37"
						  pos1="0.0, 0.0, 1.55"
						  pos2="0.0, 0, 0.8" />
			<!-- the cave troll's own: Native's crouched pos2 is "0.0, 0, 0.8" -->
			<crouched_body_capsule radius="0.37"
								   pos1="0.0, 0.0, 1.55"
								   pos2="0.0, 0, 0.6" />
		</Capsules>
		<!-- trimmed: <Flags>, as Native's human but CanClimbLadders and CanRide "false" -->
	</Monster>
```

Then the four variants, diffs through `base_monster`, which keeps every attribute you omit. Native
bases its `_settlement_slow` and `_settlement_fast` on `human_settlement`; the cave troll bases them
on `cave_troll`:

```xml
	<Monster id="cave_troll_child"
			 base_monster="cave_troll"
			 action_set="as_cave_troll_child"
			 weight="30"
			 walking_speed_limit="1.6"
			 standing_eye_height="1.20"
			 crouch_eye_height="0.70"
			 arm_length="0.6"
			 arm_weight="2.4" />
	<!-- trimmed: cave_troll_settlement, and cave_troll_settlement_fast at 1.6 -->
	<Monster id="cave_troll_settlement_slow"
			 base_monster="cave_troll"
			 walking_speed_limit="1.1" />
```

**4. Add the action sets**, with `as_human_warrior` as the base of your `as_<race>_warrior`, which
then overrides only the clips you add:

```xml
	<action_set id="as_cave_troll_warrior"  base_set="as_human_warrior">
		<action type="act_run_forward_unarmed" animation="anim_troll_run1" />
		<!-- trimmed: the rest of the cave troll's own clips -->
	</action_set>

	<action_set id="as_cave_troll_female_warrior" base_set="as_cave_troll_warrior" />
```

Then copy Native's other `as_human_*` sets under your prefix, bases renamed. The cave troll has 85,
among them `as_cave_troll_child` on `as_cave_troll_villager` (as `as_human_child` is on
`as_human_villager`) and the facegen pair below.

**5. Give each troop** `race="cave_troll"`, spelled exactly, and a `<face>` with a
`face_key_template` ([NPC character](/modding/npc_character/)).

**6. Restart the game** and fight a Custom Battle.

A race on its own skeleton carries its size in the skeleton: **measure its Monster's heights,
`arm_length` and capsule from the model**. TAOM's dwarf Monster kept the human's while the dwarf
stands about 82% as tall, so the engine seated a dwarf rider as if human-sized. A humanoid race
takes `monster_usage="human"` and a `bipedal` set, no mount actions, no `quad_movement`. Capsule,
spacing and weapons: [the battle page](/guides/custom_creature_battle/).

## The skeleton: human names, human order, human axes

A race skeleton is the human's with other lengths. Four things stay the human's.

**Names.** Keep `human_skeleton`'s 28 bone names: the Monster's bone attributes resolve by name,
and a miss is index -1, silently
([bone attributes](/guides/custom_creature_xml/#bone-attributes-resolve-by-name-and-a-miss-is-silent)).

**Order.** The Modding Kit (the Steam tool that turns FBX files into `.tpac` packages;
[FBX import](/3d/editor_fbx_import/)) stores a clip's bone tracks in FBX node order, and the engine
reads track i as bone i of the skeleton's list. Keep the human's hierarchy: its list is depth-first,
the order Blender writes ([the export mapping](/guides/custom_creature_animation/#the-export-mapping)).
`Usage` must be `human`; a Kit import arrives as `other`. The cap is
[64 bones](/guides/custom_creature_skeleton/#bone-limits).

**Grip bones.** Include `l_finger0` and `r_finger0`: the Monster hangs held items on them; clips do
not need them. Carried from the human hand, the hill troll's grips sat 0.22 m above its fist until
the artist moved them: check them against the skin.

**Axes.** A clip stores each bone's rotation in that bone's own axes, and the engine plays the
numbers as they are. The hill troll's artist rig ran each bone along +Y, rolled 180 to 280 degrees
off the human's (+X), and a human guard clip twisted its arms, shoulders and head in the Kit.

Skinning is roll-independent ([the one rule](/guides/custom_creature_skeleton/#the-one-rule)), so
**re-frame** the rig: turn every bone to the human's axes, heads kept, mesh and weights untouched.
Per bone:

1. Take the human bone's world rest frame: transpose each `RestFrame` and accumulate down the
   hierarchy ([rest-frame maths](/guides/custom_creature_skeleton/#rest-frame-maths-if-you-are-rebuilding-the-rig-in-blender)).
   Its X column runs along the bone.
2. Turn it by the rotation that carries the human bone's direction (toward its child) onto your
   bone's, and the human limb's front onto yours.
3. Keep your bone's head; set `tail = head + Y column x length` and `align_roll(Z column)`, the frame
   first mapped into Blender's axes (the hill troll's export: engine = (-x, -y, z)).

!!! note "Re-framed bones draw sideways in Blender, and that is expected"
    Blender draws a bone along its Y axis; the engine's bone axis is X. The skeleton page's
    `tail = head + world_X` draws a rig along its limbs for viewing only: the Kit stores an FBX's
    bone frames as they are.

Export rig and meshes as one FBX with `primary_bone_axis='Y'`, `secondary_bone_axis='X'`, which pass
the frames through unchanged ([exporting the rig](/guides/custom_creature_skeleton/#exporting-the-rig)).
The "X axis, -Y axis" settings quoted on the site (Armature/Skeleton, [Animations](/modding/animations/))
are for importing; TAOM has not tested them on this rig. Name each material as its Kit material: the
Kit binds by name across the module, and a same-named older material wins silently
([materials](/guides/custom_creature_skeleton/#check-materials-after-every-fbx-re-import)).

!!! tip "Optional helpers"
    [`tpac_skeleton_copy_physics.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/tpac_skeleton_copy_physics.py)
    `--reframe` computes the turned frames and missing grips;
    [`blender/export_rig_for_kit.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/export_rig_for_kit.py)
    `--bone-frames` applies them on export and checks every bone after a round trip.

## Physics: copy the human's, through the rest pose

**By hand, only the Kit route the site's Armature/Skeleton page quotes (below) and the rules in the
numbered list are documented**; the Kit's per-bone field names are not recorded, and the carry
through the rest pose and the hit-capsule fit have been done only with TAOM's tools.

Each bone has a body: a ragdoll capsule that moves the corpse, a hit capsule that weapons strike, a
mass and a body zone. A Kit import gives `Usage` `other`, empty bodies and no joints;
`human_skeleton` has 28 bodies and 34 joints, 15 typed `d6` and 19 typed `ik`.
**The `ik` joints are ragdoll constraints, not animation IK.**

A verbatim copy works only on a rig that keeps the human's axes, like the dwarf's; on any other,
capsules lie across limbs and joints hinge about the wrong axes. So carry each record through the
rest pose:

1. Turn capsule ends and joint frames by the rotation taking the human bone's direction and front
   onto the race bone's: the identity after a re-frame (take that from the re-frame's record, not
   the rig).
2. Scale the ends by the bone-length ratio along the bone and the height ratio across it.
3. Copy masses, zones, joint limits and lock modes unchanged.

**Thickness is the one thing a copy cannot supply.** Scaled by height, the hill troll's copied hit
capsules held 5.8% of its skin. Fit [hit capsules](/guides/custom_creature_battle/#three-collision-layers)
to the skin (99.4% after), then size each ragdoll radius from its hit radius at the human's ratio.

**By hand in the Kit**, open the skeleton in the skeleton editor (find it in the
[resource browser](/editor/resource_browser/)), set its `Usage` from `other` to `human` and fill each
bone's body from `human_skeleton`'s; the site's Armature/Skeleton page quotes this route, then File >
Generate joints and a save. TAOM has not tried Generate joints: whether it builds the human's 34
joints and limits is not established, so compare with `human_skeleton`.

**Then save the module in the Kit**: saving writes the package and its RuntimeDataCache entry,
`<module>/RuntimeDataCache/<package GUID>.rdc`, without which the game skips the package with no log
line ([what Save writes](/guides/custom_creature_clip_inspector/#what-save-writes-and-the-runtimedatacache)).
A fresh time on that file is the check.

**Check the physics after every re-import**: a re-import that rebuilds the skeleton empties its
physics while bones and `Usage` look right
([physics](/guides/custom_creature_skeleton/#physics-bodies-and-ragdoll-joints)).

!!! note "Not verified in game"
    In the Kit's skeleton view the hill troll's ragdoll capsules sit inside the body, its hit capsules
    around it, joints present; no in-game hit test or corpse fall has been run. That weapons meet the
    hit capsule is read from field names, and whether the game reads bodies from the package or its
    `.rdc` is not established.

??? note "For tool writers"
    * A joint's `rot` is a quaternion stored W, X, Y, Z, turning the child bone's axes into the
      joint's.
    * TaleWorlds' own `human.tpac` stores 0 under each rest frame's translation w (Kit output stores
      1), so a plain 4x4 product drops every parent's position.
    * A capsule's axis comes from the bone's direction; a fixed local x lies across a +Y rig's limbs.
    * Compare skeletons after a re-import by decompressed segments: the Kit's LZ4 output differs on
      every compile.

Optional helpers:
[`tpac_skeleton_copy_physics.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/tpac_skeleton_copy_physics.py)
carries the records;
[`blender/export_skin_for_capsules.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/export_skin_for_capsules.py)
plus [`skeleton_hit_capsules.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/skeleton_hit_capsules.py)
`fit --axis bone` fit hit capsules;
[`tpac_skeleton_dump.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/tpac_skeleton_dump.py)
prints bodies and joints.

## Face and hand morph channels

A morph channel is a shape key the engine blends at runtime. Race meshes carry them on LOD0 only.

!!! danger "A race head with no face morph channels crashes the first agent built from it"
    Give the LOD0 head, eye and mouth meshes the 101 face morph channels a race head normally
    carries. Without them the Kit writes an empty morph record, the engine's null check covers only
    its pointer, and the game crashes about a second into deployment. The v1.5.3 signature is an
    access violation at `TaleWorlds.Native.dll+0x57070C` (the offset moves with every engine update),
    in the code whose string is "No morph data found for face mesh. Can not do static morph."
    Zero-offset channels stop the crash; the face sliders then move nothing.

    The Kit never runs a face morph, so every Kit look passes
    ([debugging a native crash](/guides/custom_creature_troubleshooting/#debugging-a-native-crash)).

**Zero-offset channels by hand:** in Blender, give each LOD0 head, eye and mouth object a Basis
shape key and 101 copies of it at value 0; no reference head is needed. Working heads name channels
three ways (`shape_01` on, frame names, `Basis_0` to `Yell_100`), so TAOM's reading is that the
engine goes by order. **To check the result, count the channels: 101 each on the LOD0 head, eye and
mouth parts**, the Basis not counted. Working sliders would need a working head's 101 channels in
order; TAOM has not built one.

**Export the head's parts as separate objects.** The Kit makes one sub-mesh per FBX object,
`<mesh>`, `<mesh>.eye` and `<mesh>.mouth`; joined, the head splits by material into `.0` and `.1`,
and a mouth sharing the head's material cannot be tagged. After the import, tag them
`face_base_mesh`, `face_eye_mesh` and `face_mouth_mesh` in the Kit and save: the tags live in the
package, not `skins.xml`. TAOM's notes do not name the Kit panel that sets them. Point the skin's
`<mouth_textures>` at the new mouth's material.

**Hands.** `human_skeleton` has no finger bones beyond `finger0`, so every grip is a shape key:
**26 hand-pose channels on the LOD0 hand or arm mesh.** Without them nothing crashes; the fingers
never close on the weapon. TAOM's reverse engineering found that a clip's hand poses, 0 to 4 each,
select morph key `5L + R + 1` of 25, unchecked in the game and asserted below 5 by the Kit. That 26
is a neutral plus those 25 is likely, not proven.

* **Copy them from a hand that has them.** TAOM fitted another race's hand, from its own source art,
  onto the new one and bound it with Blender's Surface Deform modifier, which turns thick fingers
  instead of shearing them. Whether vanilla's hand meshes carry the 26, and whether TpacTool
  exports them, TAOM has not checked, so no source of such a hand is known outside a working race's
  own art.
* **Use a hand mesh, not an arms mesh**: an arms mesh's channels still move at mid-forearm and tore
  the hill troll's wrist seam. Judge the wrist in a render and in game.
* **Blender 5.2's `shape_key_add` creates keys at value 1.0**, exported as `DeformPercent`, so the
  file re-imports with every pose applied. Set each key to 0. Whether the Kit or the engine applies
  `DeformPercent` is not established.

The Kit's FBX Import Settings dialog offers skeletons, skeletal animations and morph animations; the
site's [FBX import](/3d/editor_fbx_import/) page shows its settings for hand morphs on gloves.

Hand poses and facial animation run only in the engine's human animation system. Which monsters use
it is not established, but the hill troll's hand morphs work in game.

Optional helpers: [`blender/add_face_morph_channels.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/add_face_morph_channels.py)
adds 101 zero-offset channels;
[`blender/transfer_hand_morphs.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/transfer_hand_morphs.py)
is the hand transfer above;
[`check_race_morph_channels.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/check_race_morph_channels.py)
counts channels in the files it lists.

## Clips: re-framing fixes axes, not poses

The Kit imports each FBX take as a **SkeletalAnimation master** holding keyframes. An
**AnimationClip** is a record that names a master and adds a frame range, flags, priority and the
rest; action sets name clips, never masters
([a clip is a record](/guides/custom_creature_clip_inspector/#a-clip-is-a-record-not-keyframes)).

A clip stores each bone's rotation relative to its parent, so re-framing makes a human clip bend each
joint about the right axis while keeping the human's rest relations.

**When human clips play as they are.** Preview one on your rig in the Kit's
[model viewer](/3d/model_viewer/) before binding a set. TAOM's dwarf, its bones within about 35
degrees of the human's, plays the human set as it is. On the hunched hill troll (`spine2` 54 degrees
and the neck 45 off the human's rest), every human clip pitched the head 45 to 65 degrees up and
twisted the wrists 20. No threshold is established; if your preview looks like the troll's,
**retarget every clip the race plays, human clips included.**

Human clips live in `Native/AssetPackages/animation_clips.tpac` (6,177), naming masters in
`animations.tpac` (4,052) under other names; some masters are 0-frame shells, and masters key
sparsely, so take the length from the last key.

This retarget matched its source on TAOM's 52 pack clips, feet within 2 cm by measurement; an
earlier cut was approved by eye in the Kit, and the final one has walked and fought in battle. Each
step fixed a measured fault:

1. **World-space rotation deltas**, blind to both rigs' bone axes.
2. **Rests aligned by limb direction**, never clavicles or pelvis (they dragged the shoulders back).
3. **Trunk referenced on an idle frame, limbs on the bind pose**; bind pose alone tilted the head 40
   degrees up.
4. **Twist helpers mapped**, or the forearm's roll lands on the wrist.
5. **Feet delta-only**; aligned, the toes sank 9 to 17 cm.
6. **Two-bone leg IK anchored on the source's stance from the target's hip**, the knee plane from a
   rest pole carried by the thigh; anchored on the target's rest foot, steps overshot by 19%.
7. **Root height kept**: an Unreal Engine export parks pelvis height above bind on the root.
8. **Frame 0 keyed as rest**, clip from Source 1 = 1
   ([feet above the ground](/modding/animations/#feet-above-the-ground) agrees).
9. **Travel scale from the retarget's stride report**; a guessed 1.377 against the true 1.5578
   would have skated the feet 12%.

After the Kit import, masters can arrive naming no skeleton, silently: set each one's. Clip names
hold [63 characters](/guides/custom_creature_clip_inspector/#clip-names). A clip cloned from a
vanilla template carries its Blends with animation, Blends with action and Loading Type: check all
three ([the melee page](/guides/custom_creature_melee/#the-first-swing-crash)).

In battle the hill troll walks and fights on these clips; some staff guard, defend and bash actions
and some hit reactions still use vanilla clips. Its own swings: see
[the action set](#the-action-set-standalone-or-inherited).

Optional helpers: [`read_anim_keyframes_tpac.ps1`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/read_anim_keyframes_tpac.ps1)
`-ByClip` resolves clips to masters and dumps keys;
[`blender/retarget_mannequin_to_human.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/blender/retarget_mannequin_to_human.py)
is the retarget above;
[`wire_anim_master_skeletons.ps1`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/wire_anim_master_skeletons.ps1)
sets empty master skeletons;
[`gen_troll_anim_clips.ps1`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/gen_troll_anim_clips.ps1)
cuts clips from vanilla templates. The PowerShell ones default to one machine's paths.

## The action set: standalone or inherited

An action is a named engine action such as `act_release_overswing_2h`, declared in
`action_types.xml` as `<action name="..." type="...">`. Its action type is the kind in that `type`
attribute (`actt_release_melee` here), and its action code is the index the engine gives the name at
runtime. An action set binds actions to clips for one skeleton; in `action_sets.xml` the attribute
naming the action is also called `type`
([the XML page](/guides/custom_creature_xml/#action_typesxml-and-action_setsxml)).
`base_set` copies the base's skeleton, movement system and every binding at parse time (TAOM's
reverse engineering of v1.5.3), so a child of `as_human_warrior` keeps up with the engine. A
standalone set (own `skeleton=`, no `base_set`) inherits nothing and
[rots](/guides/custom_creature_xml/#standalone-action-sets-rot-across-engine-updates).

**What is proven for a race on its own skeleton is a standalone set at full parity with Native's
`as_human_warrior`**: TAOM's dwarf, and the hill troll's 4,700 actions. Re-check after every update.

!!! note "Not verified in game"
    A child of `as_human_warrior` declaring its own `skeleton=` would inherit new actions for free,
    but TAOM has not tested one on a race skeleton.

A set can bind another skeleton's clips: the hill troll's binds 3,909 human clips beside 792 of its
own and plays in game, so skeleton identity appears unenforced; whether bone counts must match is
not established.

**Character creation** asks for `as_<race>_facegen` and `as_<race>_female_facegen`. Native's pair is
the model: `as_human_facegen` on `base_set="as_human_warrior"` declares 103 actions of its own,
`as_human_female_facegen` on `as_human_facegen` declares 24. Copy the pair under your prefix, the male
set on your combat set. That base is fine; a short list is not: a slim set with only the parent
actions fixed the parent menu and left later stages lying down. **The set you inherit from must
itself bind the action.**

**Off the battlefield** the engine plays these sets too: inventory previews idle on
`act_inventory_idle*`, a conversation mission uses the `_poses` set, victories play `act_cheer_*`.
A reused clip needs the flags of the vanilla clip bound to that action (Native binds `act_inventory_idle` to
`inventory_idle`), not the battle idle's: the hill troll's reused idles lack the vanilla clips'
`cyclic`, and these screens set the action once, so by TAOM's reading they play once and stop (not
yet seen in game). A valid set is not a posed character:
check the action binds a clip (`MBActionSet.GetAnimationName`).

**Four families go through the engine's melee attack table**: `act_release_*`,
`act_quick_release_*`, `act_blocked_*` and `act_quick_blocked_*`. A race clip bound there needs a
row, or the first swing crashes: bind vanilla clips first
([melee attack clips](/guides/custom_creature_melee/)).

The hill troll self-keys its own release and blocked clips (each names itself in Blends with
animation, which gives it a row) and binds them to 32 two-handed actions of those families. In
Custom Battle it entered the twelve quick release and quick blocked ones (overswing, slash left,
slash right, both stances) with no crash. TAOM's trace names the clip bound to each action, not the
frames drawn, so the motion has not been confirmed by eye; all twelve clips sit at Loading Type 2.
The full-length release and blocked actions have not come up in a test.

**Set Loading Type 0, Always keep in memory, on any clip you bind to these four families.** It is
the conservative choice, and all 175 of vanilla's self-keyed clips are at 0. It is not a proven
requirement: of vanilla's 347 unkeyed clips at 2, Native's `action_sets.xml` binds 254 directly to
actions that animate in vanilla play (conversation gestures, tavern drinking, the cat and dog
gaits), so 2 does not by itself stop a directly bound clip from playing. What plays through a
self-keyed row at 2 is not established
([the fields](/guides/custom_creature_clip_inspector/#the-fields-one-by-one)).

Optional helpers: [`patch_dwarf_action_parity.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/patch_dwarf_action_parity.py)
fills a standalone set to parity;
[`audit_action_set_parity.py`](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/audit_action_set_parity.py)
lists humanoid sets short of Native's. Both default to TAOM's layout.

## Next

* [Melee attack clips](/guides/custom_creature_melee/): the first-swing crash.
* [The clip inspector](/guides/custom_creature_clip_inspector/): each clip field in game.
* [Big creatures in battle](/guides/custom_creature_battle/): collision, size, formations.
