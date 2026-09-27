# Custom Creatures

How to add a new creature to Bannerlord: a mesh, a skeleton, animation clips, the XML that binds
them together, and the item that makes it rideable.

Two reading orders, by what you are building.

**A mount** (a creature a troop rides):

1. [Custom Creature: the skeleton](/guides/custom_creature_skeleton/)
2. [Custom Creature: the animation clips](/guides/custom_creature_animation/)
3. [Custom Creature: the XML](/guides/custom_creature_xml/)
4. [Custom Creature: the clip inspector](/guides/custom_creature_clip_inspector/)
5. [Custom Creature: big creatures in battle](/guides/custom_creature_battle/)
6. [Custom Creature: troubleshooting](/guides/custom_creature_troubleshooting/)

**A humanoid race** (a two-legged soldier with its own size or proportions):

1. [Races](/modding/races/), for the basic `skins.xml` race
2. [Custom Creature: a humanoid race on its own skeleton](/guides/custom_creature_race/)
3. [Custom Creature: the clip inspector](/guides/custom_creature_clip_inspector/)
4. [Custom Creature: melee attack clips](/guides/custom_creature_melee/)
5. [Custom Creature: big creatures in battle](/guides/custom_creature_battle/)
6. [Custom Creature: troubleshooting](/guides/custom_creature_troubleshooting/)

A race on its own skeleton also needs [the skeleton page](/guides/custom_creature_skeleton/) before
step 2. Both look things up in [Custom Creature: reference tables](/guides/custom_creature_reference/).

Related pages you will need: [Armature/Skeleton](/3d/armature_skeleton/) for how rigging works at
all, [Animations](/modding/animations/) for playing a clip from C#, and
[TpacTool](/resources/tpactool/) for reading the engine's own assets.

!!! note "Version"
    Measured on **Bannerlord v1.4.5 to v1.5.3**. Engine-code findings come from TAOM's reverse
    engineering of the v1.5.3 game and Modding Kit DLLs; native crash offsets move with every engine
    update, and each page marks facts measured on an older version. v1.4.6 changed which data mistakes
    are survivable: see [The 1.4.6 rule](/guides/custom_creature_xml/#the-146-rule) before porting
    anything older.

## What a creature is, to the engine

A creature is not one thing. It is five, and they are registered in different places by different
mechanisms:

| Piece | Lives in | What it does |
|---|---|---|
| **Skeleton** | a binary `.tpac` asset | the bones, plus collision bodies and ragdoll joints |
| **Animation clips** | binary `_anm.tpac` assets | the poses over time |
| **`Monster`** | `monsters.xml` | weight, hit points, capsules, and the bone-name map |
| **`action_set`** | `action_sets.xml` | binds an action name to a clip, for one skeleton |
| **`monster_usage_set`** | `monster_usage_sets.xml` | tells the engine which action to fire, when |

If the creature is rideable there is a sixth: an `Item` of `Type="Horse"` whose `Horse` component
names the Monster. That is the whole of it. **A rideable creature is a vanilla cavalry spawn.** You
give a troop a mount item, the engine does the rest: mounting, dismounting, reins, charge damage,
the campaign map icon. You do not need to patch spawning, and you do not need a second "combatant"
agent for the creature itself.

!!! tip "Say the last part out loud before you start"
    Building a creature as a detached non-mount agent is the single most expensive wrong turn
    available here. TAOM built that architecture twice and deleted it twice. If your creature
    carries a rider, make it a mount.

!!! note "Terms used across these pages"
    * **Usage** has three meanings: a skeleton's `Usage` (`horse`, `human` or `other`; an import
      arrives as `other`); a **clip usage**, a typed record on a clip such as `quad_movement`; and a
      Monster's `monster_usage`, which names the **monster usage set** that tells the engine which
      action to fire, and when.
    * **Action**: a named engine action such as `act_release_overswing_2h`, declared in
      `action_types.xml` as `<action name="..." type="...">` and bound to a clip per action set in
      `action_sets.xml` (where the attribute naming the action is also called `type`).
    * **Action type**: the kind in that declaration's `type` attribute, such as `actt_kick` or
      `actt_defend_shield`, which the engine branches on.
    * **Action code**: the runtime index the engine gives an action name.
    * **The human animation system**: the engine's native animation system for humans (horses have
      their own). Only it applies facial animation and hand poses; which monsters run it is not
      established.
    * **TAOM's reading**: a conclusion from TAOM's reverse engineering, not yet confirmed in game.
    * **Fab**: Epic's asset marketplace (fab.com), where TAOM bought its cave troll and Animalia clips.
    * **LOD0**: a mesh's full-detail level; LOD1 and later take over with distance
      ([Create LODs](/3d/create_lods/)). Race meshes carry morph channels on LOD0 only.
    * **`d6`**: a ragdoll joint type with six lock states, each `locked`, `limited` or `free`; the
      others are `hinge` and `ik`.
    * **BodyProperty**: a face range (a minimum and a maximum) each troop's face is rolled from.
      Its `key`, the **body key**, is the face itself: 128 hex characters from the in-game face editor.
    * **`DeformPercent`**: the FBX field holding a shape key's value; whether the Kit or the engine
      applies it is not established.

## There are two paths, and one is much cheaper

The expensive path is not always the right one. Decide this first, because it changes everything
downstream.

### The reskin path

Your new mesh is skinned to a skeleton the engine **already has**, bone for bone. You author no
animation at all. The whole creature is a handful of XML attributes.

This works because `Monster.Deserialize` copies `Flags`, `ActionSetCode`, `FemaleActionSetCode`,
`MonsterUsage` and every capsule field from the base monster, and every attribute you do **not**
name keeps its inherited value. Its defaults are guarded behind a "has a base_monster" check, so
naming a base turns the whole record into a diff.

TAOM's war ram started as exactly this. Here is its entire Monster definition as a pure reskin:

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

That inherits `Mountable`, `CanRear`, `RunsAwayWhenHit`, `CanCharge`, `CanWander`,
`family_type="1"`, `monster_usage="horse"`, `num_paces="6"`, every bone name, the ground-slope
block, and all twelve rein attributes. For free, and correctly.

A reskin that needs one clip stays on this path: the war ram's head-butt lives in a thin set,
`action_set="as_war_ram"`, that inherits `as_horse` and adds one action
([the XML](/guides/custom_creature_xml/#the-minimum-if-you-are-reskinning)). With no clip, keep `as_horse`.

!!! warning "A thin set needs its `_map` child"
    The campaign map builds a mounted leader's mount from the Monster's set name plus `_map`, and the
    engine code throws when that set is missing. Build it on the donor's: `as_war_ram_map` inherits
    `as_horse_map`. A `_town_and_village` child is optional.

!!! warning "A reskin inherits the donor's behaviour, not just its animations"
    The property that makes it cheap is the same one that couples it. Your creature now shares an
    action vocabulary with the **engine**, so "our code never fires this action" stops meaning
    "nothing fires it". This has a specific, nasty failure mode covered in
    [the reskin trap](/guides/custom_creature_xml/#the-reskin-trap). Read it before you bind an
    attack.

### The bespoke path

A new skeleton, new clips, a new action set and a new monster usage set. You need this when the
creature's proportions or leg count genuinely differ: a spider, an elephant, a wolf-sized quadruped
with a different spine.

This is weeks of work rather than an afternoon, and nearly every crash in the
[troubleshooting page](/guides/custom_creature_troubleshooting/) belongs to it.

### Which one

| If | Then |
|---|---|
| Your creature is roughly horse-shaped and horse-sized | **Reskin.** Skin it to `horse_skeleton` and stop. |
| It is horse-shaped but a very different size | **Reskin**, and scale it. Read the `body_length` warning in [the XML page](/guides/custom_creature_xml/#size). |
| It has a different number of legs, or a radically different spine | **Bespoke.** |
| It walks on two legs with its own proportions (a race, not a mount) | **Own skeleton, human bone names and axes.** See [a humanoid race on its own skeleton](/guides/custom_creature_race/). |
| You want it to attack with something other than a kick | **Bespoke**, or author one clip onto the existing rig, whose only attack clip is the kick. **A clip does not attack by itself:** code, such as a behaviour tree, has to play it and apply the blow. The only attack the engine fires on its own is the usage set's `kick_action`, and no test records a new creature's own kick action firing. See [scripted creature attacks](/guides/custom_creature_battle/#scripted-creature-attacks). |
| You are not sure | **Reskin first.** Get something walking in game, then decide. A working creature is a much better place to iterate from than a half-built rig. |

### The creatures on these pages

| Creature | Shape | Path | Size |
|---|---|---|---|
| War ram | a dwarf's war goat | **reskin** of `horse_skeleton`, one head-butt clip in a thin set | 1x: 2.27 m to the horns |
| Great elk | an elk | **reskin** playing the ram's set, the head-butt as an antler charge | 1.1x: 3.38 m to the antler tips |
| Animalia elk and moose | from purchased Fab packs | **reskin** playing the pack's own clips, retargeted | elk 1x, moose 1.5x |
| Giant spider | eight legs | **bespoke**: own 62-bone skeleton, clips and sets | 1x to 1.25x by skin |
| War elephant | an elephant with a howdah crew | **bespoke**: 60-bone skeleton and clips from Artem's ADOD_Beasts | 1x |
| Mumakil | the war elephant with a war tower | **bespoke**: the elephant's skeleton, sets and clips | 3x |
| Chariot | two horses and a cart on one skeleton | **bespoke**: one 60-bone skeleton, data only | not recorded |
| Cave troll | a large humanoid | **race** on `human_skeleton`, scaled by its skin | about 1.9x |
| Hill troll | a hunched humanoid | **race** on its own 28-bone skeleton: human names, order and axes | 3.6 m tall |
| Dwarf | a short humanoid | **race** on its own skeleton, human axes | about 82% of human height |

Byak0's warg, TAOM's control creature, is under Acknowledgements.

## What you need installed

| Tool | For | Where |
|---|---|---|
| **Mount & Blade II: Bannerlord Modding Kit** | Importing FBX, compiling `.tpac`, the model viewer | Steam, under Library → Tools |
| **Blender** | Rigging and authoring clips | blender.org |
| **TpacTool** | Reading the engine's own skeletons and clips | [TpacTool](/resources/tpactool/) |

The Modding Kit is not listed on this wiki's [Tools](/resources/tools/) page but you cannot do any
of this without it. It is a separate free download in the Steam Tools library, and it installs a
second copy of the game with the editor enabled.

## Where the files actually are

This trips people up, so it is worth stating plainly.

**Skeletons are not XML.** There is no `skeletons.xml` anywhere, in any module. Skeletons are
binary assets, and the vanilla ones all live in one file:

```
Modules/Native/AssetPackages/skeletons.tpac
```

The animation XML lives in `Modules/Native/ModuleData/`: `monsters.xml`, `action_sets.xml`,
`action_types.xml`, `monster_usage_sets.xml`, `movement_sets.xml`. Note that `SandBox` ships a
`monsters.xml` too, and it defines exactly one thing (`cover_cow`), so read `Native`'s for the real
vocabulary.

Mount items are elsewhere again, in `Modules/SandBoxCore/ModuleData/items/horses_and_others.xml`.

## Acknowledgements

The lessons here came out of building creatures for **TAOM (Tales From the Age of Men)**, a Lord of
the Rings total conversion. Most of them were learned by getting something wrong in a way that took
a day to understand, which is the only reason they are worth writing down.

Two creature authors' work taught us most of what is on these pages, and both are named with
permission of the relationship under which we used their assets:

* **Artem**, author of **ADOD_Beasts**, whose war elephant TAOM licensed. The elephant is the
  reference for a large quadruped, and Artem is also the source of the
  [Custom Mount](/guides/custom_mount/) notes already on this wiki.
* **Byak0**, author of **Alliance** and **Alliance.Wargs**. The warg is the reference
  implementation for a rideable creature with a bespoke skeleton, and is the known-good control
  we measured almost everything against.

Corrections are welcome. Several claims in the first drafts of TAOM's own internal notes turned out
to be wrong, and where that happened these pages say so rather than quietly dropping them, because
a confidently-stated wrong number costs the next person a day.
