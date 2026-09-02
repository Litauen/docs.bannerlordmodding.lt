# Custom Creatures

How to add a new creature to Bannerlord: a mesh, a skeleton, animation clips, the XML that binds
them together, and the item that makes it rideable.

* [Custom Creature: the skeleton](/guides/custom_creature_skeleton/)
* [Custom Creature: the animation clips](/guides/custom_creature_animation/)
* [Custom Creature: the XML](/guides/custom_creature_xml/)
* [Custom Creature: troubleshooting](/guides/custom_creature_troubleshooting/)
* [Custom Creature: reference tables](/guides/custom_creature_reference/)

Related pages you will need: [Armature/Skeleton](/3d/armature_skeleton/) for how rigging works at
all, [Animations](/modding/animations/) for playing a clip from C#, and
[TpacTool](/resources/tpactool/) for reading the engine's own assets.

!!! note "Version"
    Everything here was measured against **Bannerlord v1.4.8**. Where v1.4.5 and v1.4.6 behave
    differently it is called out, because v1.4.6 changed which data mistakes are survivable. See
    [The 1.4.6 rule](/guides/custom_creature_xml/#the-146-rule) before porting anything older.

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

TAOM's war ram is this. Here is the entire Monster definition:

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
| You want it to attack with something other than a kick | **Bespoke**, or author one clip onto the existing rig. The horse rig has no attack animation. |
| You are not sure | **Reskin first.** Get something walking in game, then decide. A working creature is a much better place to iterate from than a half-built rig. |

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
