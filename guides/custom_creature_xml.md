# Custom Creature: the XML

The `Monster` record, the action set, the monster usage set, how to register them so the engine
actually loads them, and the item that makes the creature rideable.

Part of the [Custom Creatures](/guides/custom_creatures/) guide.

!!! note "Version"
    Measured against **Bannerlord v1.4.8**. The [1.4.6 rule](#the-146-rule) below changes which
    mistakes in this file are survivable, so check your target version before copying anything from
    an older mod.

## `monsters.xml`

### The minimum, if you are reskinning

```xml
<Monsters>
	<Monster
		id="taom_war_ram"
		base_monster="horse"
		action_set="as_horse"
		weight="320"
		hit_points="160"
		jump_acceleration="7.5"
		relative_speed_limit_for_charge="4.0" />
</Monsters>
```

`Monster.Deserialize` copies `Flags`, `ActionSetCode`, `FemaleActionSetCode`, `MonsterUsage` and
every capsule field from the base, and every attribute you do not name keeps its inherited value.
So this inherits `Mountable`, `CanRear`, `RunsAwayWhenHit`, `CanCharge`, `CanWander`,
`family_type="1"`, `monster_usage="horse"`, `num_paces="6"`, every bone name, the ground-slope
block, and all twelve rein attributes.

!!! tip "`<Flags>` is only reassigned if you supply a `<Flags>` child element"
    A self-closing `<Monster ... />` does not have one, so the base's flags survive intact. This is
    what makes the one-liner safe.

### The full form, for a bespoke creature

TAOM's warg, which is the reference implementation for a rideable creature on its own skeleton:

```xml
<Monsters>
	<Monster
		id="warg"
		action_set="as_warg"
		monster_usage="warg"
		weight="500"
		hit_points="80"
		absorbed_damage_ratio="1.0"
		num_paces="6"
		jump_acceleration="8"
		sound_and_collision_info_class="warg"
		rider_camera_height_adder="-0.5"
		rider_body_capsule_height_adder="0"
		rider_body_capsule_forward_adder="0"
		standing_chest_height="1.30"
		standing_pelvis_height="1.40"
		standing_eye_height="1.40"
		rider_eye_height_adder="1.7"
		eye_offset_wrt_head="0.13, -0.15, 0.0"
		jump_speed_limit="4.5"
		relative_speed_limit_for_charge="4"
		family_type="1"
		rider_sit_bone="Spine1_M"
		rein_handle_bone="Spine3_M"
		rein_collision_1_bone="Spine2_M"
		rein_collision_2_bone="Spine2_M"
		neck_root_bone="Neck_M"
		pelvis_bone="Root_M"
		right_upper_arm_bone="Scapula_R"
		left_upper_arm_bone="Scapula_L"
		terrain_decal_bone_0="Fingers2_L"
		terrain_decal_bone_1="Fingers2_R"
		front_bone_to_detect_ground_slope_index="2"
		back_bone_to_detect_ground_slope_index="4"
		bones_to_modify_on_sloping_ground_0="Neck1_M"
		bones_to_modify_on_sloping_ground_1="Scapula_L"
		bones_to_modify_on_sloping_ground_2="Scapula_R"
		bones_to_modify_on_sloping_ground_3="Hip_L"
		bones_to_modify_on_sloping_ground_4="Hip_R"
		body_rotation_reference_bone="Spine3_M"
		head_look_direction_bone="Neck1_M"
		thorax_look_direction_bone="Spine3_M"
		rein_handle_left_local_pos="0,-1,0.15"
		rein_handle_right_local_pos="0,-1,-0.15">
		<Capsules>
			<body_capsule
				radius="0.4"
				pos1="0, 1.15, 1.15"
				pos2="0, -0.57, 1.15" />
		</Capsules>
		<Flags
			Mountable="true"
			CanRear="true"
			RunsAwayWhenHit="true"
			CanCharge="true"
			CanWander="true" />
	</Monster>
</Monsters>
```

### The attributes that are not free choices

| Attribute | Value | Why |
|---|---|---|
| `num_paces` | **6** | the mount machinery indexes the gallop pace at 5 |
| `family_type` | **1** for anything rideable | family 1 carries vanilla's complete rider-death, dismount and rider-fall surface. A TAOM creature tried family 11; no such surface exists for it |
| `<Flags>` | `Mountable`, `CanRear`, `RunsAwayWhenHit`, `CanCharge`, `CanWander` | and see the warning below |

Vanilla's own `family_type` legend, quoted from `SandBox/ModuleData/monsters.xml`:

```xml
<!-- family_type:
  0 for human
  1 for horse
  2 for camel
  3 for cow
  4 for goose
  5 for hog
  6 for sheep
  7 for hare
  -->
```

!!! danger "Never declare `CanAttack` on a Mountable monster"
    It activates an engine attack-AI path that no mount takes. On v1.4.6 this was one of three
    crashes in a single day: a null dereference inside the native `set_attack_entity` during a
    charge.

    Your creature's attacks come from your own code or from the usage set, not from this flag.

### Bone attributes resolve by name, and a miss is silent

Every `*_bone` attribute is looked up **by name** in the skeleton's bone array. A name that does not
match yields index **-1**. It does not error, it does not warn, and the feature that bone drives
simply stops working.

A -1 `rider_sit_bone` seats the rider at the world origin, which reads in game as "the rider is
floating at his own feet".

Copy bone names out of the skeleton itself, not out of another creature's XML, and preserve leading
spaces if the export produced them.

### The rein attributes, and an open question

`Monster.Deserialize` reads **twelve** rein attributes: the four in the warg above plus
`rein_collision_body`, `rein_head_bone`, `rein_head_left_attachment_bone`,
`rein_head_right_attachment_bone`, `rein_left_hand_bone`, `rein_right_hand_bone`, `rein_skeleton`,
and the two `rein_handle_*_local_pos` vectors.

Measured across all 94 `<Monster>` definitions in one full install: **in vanilla, "is Mountable" and
"carries all twelve rein attributes" are the same set, with no exceptions.** Monsters that use
`base_monster` carry zero of their own and inherit all twelve.

TAOM's own bespoke creatures declare five or zero, and have not crashed. But that is an untested
risk rather than a cleared one, and v1.4.8 changed the native rein path that runs on mounted-agent
death. **If you are declaring a Monster from scratch, declare all twelve.** If you are reskinning,
`base_monster` gives them to you for nothing, which is one more argument for the cheap path.

## `action_types.xml` and `action_sets.xml`

An action type is a name plus a **type**. An action set binds that name to a clip, for one skeleton.

```xml
<action_set id="as_warg" skeleton="skeleton_warg" movement_system="quadrupedal">
	<action type="act_warg_walk_forward" animation="warg_walk" />
	...
</action_set>
```

Two required siblings and one that is not:

* **`as_<creature>_map` is required.** The campaign map builds its mount visual through
  `MBGlobals.GetActionSet(monster.ActionSetCode + "_map")` at three unguarded call sites. Author it
  as `base_set="as_<creature>"` with no actions.
* **`as_<creature>_town_and_village` is NOT required.** This is worth stating because the opposite
  claim circulates, including in TAOM's own older notes. On v1.4.8 no managed code anywhere appends
  that suffix, and vanilla's `as_camel` ships without one.
* The **rider partial**, an `<action_set id="as_human_warrior">` fragment carrying your
  `act_<mount>_*` rider actions, **must sit at the top of the file**. `base_set` inheritance
  snapshots at definition time, so a partial defined later is not seen by sets defined earlier.

!!! warning "A root-level `<action>` boots the client and kills a dedicated server"
    An `<action>` element parented directly by `<action_sets>` instead of by an `<action_set>` loads
    fine in single player and throws during element merging on a dedicated server, taking it down at
    boot.

### Standalone action sets rot across engine updates

An action set with its own `skeleton=` and **no `base_set`** inherits nothing. Every action type the
engine gains after you authored it is simply absent.

One TAOM race set was seeded from Native 1.3's action list. By Native 1.4.6 it was missing **423
active types**: all the water actions, 32 hit-reaction staggers, several weapon stances, and the
naval set. A player walked into water, the engine requested `act_dive_idle_unarmed`, the set did not
have it, and the game crashed.

Sets that declare `base_set="as_human_warrior"` are immune, because they inherit. **After every
engine update, diff your standalone sets against Native's.** Parse with an XML reader rather than
grepping: Native carries roughly 126 commented-out actions and a raw grep counts them.

## `monster_usage_sets.xml`

This is what tells the engine which action to fire for which runtime state.

```xml
<monster_usage_set id="warg"
                   rear_action="act_warg_rear"
                   rear_damaged_action="act_warg_rear_damaged"
                   dash_action="act_warg_dash"
                   kick_action="act_warg_kick"
                   fast_quick_stop_action="act_warg_quick_stop_when_fast"
                   quick_stop_action="act_warg_quick_stop"
                   hit_object_action="act_warg_hit_object"
                   hit_object_falling_action="act_warg_hit_object_while_falling"
                   jump_start_action="act_warg_jump_start">
	<monster_usage_upper_body_movements>
		<monster_usage_upper_body_movement pace="0" direction="none"
		                                   turn_direction="right"
		                                   action="act_warg_turn_right_head" />
		...
	</monster_usage_upper_body_movements>
	...
</monster_usage_set>
```

### The 1.4.6 rule

**Bannerlord 1.4.6 rewrote the native usage and AI lookups so that a missed key is an access
violation.** Shipping builds compile out the asserts, so the miss path dereferences the end sentinel
or returns garbage that flows into a pointer or index slot. 1.4.5 tolerated the same gaps, which is
why data that worked for years starts crashing after an update.

The rule that follows:

> **A missing key crashes. An extra row is inert. So make every table TOTAL over the key vocabulary
> the parser accepts, not over the keys vanilla happens to use.**

Concretely, the jump table. The parser accepts **nine** directions (`front`, `front_left`,
`front_right`, `none`, `left`, `right`, `back`, `back_left`, `back_right`). Vanilla files cover ten
rows total, because vanilla riders never turn mid-jump. A creature driven by your own AI does.

Nine directions across start, loop and end states is **45 rows**. Write all 45.

Two more from the same day:

* `jump_start_action` must be typed **`actt_dash`**, never `actt_jump`.
* Every pace needs its `direction="none" turn_direction="none"` reference row, or agent creation
  divides by zero natively.

## Registration: two mechanisms, not interchangeable

This is where a lot of quiet failure lives.

**Managed types go in `SubModule.xml`:**

```xml
<XmlName id="Monsters" path="Monsters/LOTR/lotr_monster_warg"/>
```

**Native animation XML goes in `ModuleData/project.mbproj`, and only the standard ids work:**

```xml
<file id="soln_action_sets"        name="ModuleData/action_sets.xml"        type="action_set" />
<file id="soln_action_types"       name="ModuleData/action_types.xml"       type="action_type" />
<file id="soln_monster_usage_sets" name="ModuleData/monster_usage_sets.xml" type="monster_usage_set" />
<file id="soln_monsters"           name="ModuleData/monsters.xml"           type="monster" />
```

!!! danger "A custom `soln_*` id is silently ignored"
    TAOM once registered a creature's files as `soln_spider_action_sets` and similar. They never
    loaded. The action set was never registered, the usage-set index resolved to -1, and native
    agent creation hit a divide by zero on the first spawn.

    Nothing logged. The XML was correct, well-formed, present on disk, and dead.

    Use the standard ids. You can list the same id more than once for extra files.

!!! warning "Never add a second `soln_action_sets` row"
    Each one triggers another element-merge pass. Two of them throws a `KeyNotFoundException` during
    startup.

## Making it rideable

A mount is an item. That is the whole mechanism.

```xml
<Item id="taom_war_elephant" name="{=taom_war_elephant}War Elephant"
      mesh="elephant_mesh" subtype="horse" item_category="war_horse"
      weight="999" is_merchandise="false" Type="Horse">
  <ItemComponent>
    <Horse monster="Monster.taom_war_elephant" maneuver="20" speed="20"
           charge_damage="350" body_length="100" is_mountable="true" extra_health="0">
      <AdditionalMeshes/><Materials/>
    </Horse>
  </ItemComponent>
  <Flags Civilian="true"/>
</Item>
```

Give that item to a troop in its Horse equipment slot and you have cavalry.

### Size

`body_length` scales the agent: the engine calls `SetInitialAgentScale(0.01f * BodyLength)`, so 100
is identity.

!!! danger "`body_length` scales the RIDER too, not just the mount"
    `EquipmentIndex.Horse` and `EquipmentIndex.ArmorItemEndSlot` are the same value, the scale block
    has no "is this a mount" guard, and it runs for the rider as well as the mount while the Horse
    item is still in the rider's spawn equipment.

    So a `body_length` of 300 gives you a giant creature **and** a giant rider. If you want a large
    creature, prefer authoring the mesh at its real size and leaving `body_length` at 100.

### The campaign map

A mount is only visible on the campaign map when a **lord** rides one. Party visuals call
`AddCharacterToPartyIcon` exactly once, for the party's visual leader. Troops contribute nothing, so
a creature that only equips regular troops will never appear on the map no matter what you do to its
assets.

## The reskin trap

The most valuable thing on this page, because TAOM got it wrong twice, the second time while fixing
the first.

**A reskin inherits the donor's `monster_usage`, and therefore the donor's behaviour.** Your
creature now shares its action vocabulary with the engine, so "our code never fires this action"
stops implying "nothing fires this action".

Two concrete failures, both on the horse rig:

**`act_horse_rear` is typed `actt_rear`.** The inherited `horse` usage set declares
`rear_action="act_horse_rear"`, so the engine fires it on every damaged mount, and `Agent.Mount`
**refuses a mount whose channel-0 action type is `Rear`**. Bind your creature's attack to it and the
creature goes unmountable in the middle of a fight.

**`act_horse_strike_front` and `_back` are typed `actt_mount_strike`,** which sits inside the band
`Agent.IsInBeingStruckAction` reads as **being struck**. The underlying clips are literally named
`horse_hit_from_front` and `horse_hit_from_back`. Bind an attack to those and the creature flinches
as though it has been hit, at the exact moment it deals damage.

!!! note "The fact underneath both"
    **Vanilla horses have no attack animation at all.** They deal damage by charge collision. So
    `monster_usage_strikes` is the mount's hit-**reaction** table, not an attack table, and reading
    it as a list of attacks is the mistake.

    The horse rig's only genuinely offensive action is `act_horse_kick`, typed `actt_kick`. If you
    need a creature on the horse rig to attack with anything else, **the clip does not exist and you
    must author it.**

### The rule that generalises

Before binding any action to your creature's behaviour, establish three things:

1. Its **type**, from `action_types.xml`.
2. Whether the creature's `monster_usage` set names it in a verb slot or a table, which means the
   **engine** fires it too.
3. Whether the engine **branches** on that type anywhere.

"Does this action name resolve" is not the same question as "what does the engine do when this
action is active", and only the second one matters.

## Next

[Troubleshooting](/guides/custom_creature_troubleshooting/) when it does not work, and
[the reference tables](/guides/custom_creature_reference/) for action types and animation flags.
