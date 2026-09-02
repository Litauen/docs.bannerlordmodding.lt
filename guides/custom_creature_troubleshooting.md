# Custom Creature: troubleshooting

Symptom to cause, for custom creatures and mounts. Every entry here is a real failure that was
root-caused, not a guess.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. For crashes with no creature
involved, see [How to report a crash](/guides/how_to_report_a_crash/) and
[Advanced stacktrace analytics](/guides/advanced_stacktrace_analytics_of_crash_reports/).

!!! note "Version"
    Measured against **Bannerlord v1.4.8**. Several of these only became crashes in **1.4.6**; see
    [The 1.4.6 rule](/guides/custom_creature_xml/#the-146-rule).

## Quick index

| Symptom | Likely cause |
|---|---|
| Crash in **every** mount context at once: inventory thumbnail, tableau, deployment | [Missing `quad_movement`](#crash-in-every-mount-context-at-once) |
| Divide by zero on first spawn | [The XML never registered](#divide-by-zero-on-the-first-spawn) |
| Assert about `rglVec3` during asset load | [A tpac writer zeroed the TOC size](#assert-about-rglvec3-while-loading-packages) |
| Startup assert `px != nullptr`, after a texture "compiled image" line | [Loose and cooked assets both claim one name](#startup-assert-in-rglintrusive_ptrh151) |
| Crash on charge, only since 1.4.6 | `CanAttack` on a Mountable monster |
| Crash when the creature jumps, especially off terrain | [Incomplete jump table](#crash-when-the-creature-jumps) |
| Rider spawns with **no mount**, no crash | [The skeleton was dropped from the mesh tpac](#the-rider-spawns-with-no-mount) |
| Creature slides along the ground, legs frozen | [An action resolved to `act_none` on channel 0](#the-creature-slides-with-its-legs-frozen) |
| Creature invisible in battle, fine in a UI preview | [Materials lost on FBX re-import](#invisible-in-the-world-fine-in-a-preview) |
| All colour variants render the same colour | Materials lost on FBX re-import (same cause) |
| Rider floats at his own feet | `rider_sit_bone` name does not match, so it resolved to -1 |
| Creature becomes unmountable mid-fight | An attack was bound to an `actt_rear` action |
| Creature flinches while dealing damage | An attack was bound to an `actt_mount_strike` action |
| Character renders in bind pose in UI tableaux | The action set is valid but binds no clip for that action |
| Crash when a unit walks into water | A standalone action set is missing the dive actions |
| Dedicated server dies at boot, client is fine | A root-level `<action>` element |
| `KeyNotFoundException` at startup | A duplicate `soln_action_sets` row |
| Enormous memory use for one creature | Texture dimensions not divisible by 4 |
| Animation FBX exports at ~0.06 MB | [The bake found no keyframes](#the-animation-fbx-is-tiny) |
| Kit says "Item with same name already exists" | [That is correct behaviour](#the-kit-refuses-a-duplicate-name) |
| Kit clip reports `Size in KB = 0` and will not save | [The clip was renamed inside the Kit](#a-clip-reports-zero-size-and-will-not-save) |

## Crash in every mount context at once

**Signature.** Access violation at offset `+0x10`, in `Skeleton.TickAnimations` or
`GetWalkSpeedLimitOfMountable`. It fires in the inventory thumbnail, the character tableau **and**
mission deployment, which is the distinguishing feature: three unrelated code paths breaking at once
means the data they share is poisoned, not that any of them is wrong.

**Cause.** A gait clip compiled without the `quad_movement` clip usage, in an action set declared
`movement_system="quadrupedal"`. The set builds a null native gait structure and the first tick
dereferences it.

**Why it survives testing.** The clip compiles without error and plays correctly on a detached,
non-mount agent. It only detonates on quadruped mount machinery.

**Secondary tell.** Resolving an unbound action through the poisoned set returns a runtime-synthesised
garbage name, shaped like `1002467048434979358_0`.

**Fix.** [Add the tag and step points](/guides/custom_creature_animation/#quad_movement-or-the-six-hour-crash).
It is a Clip **usage**, not a Flag.

## Divide by zero on the first spawn

**Signature.** A divide by zero inside native agent creation, the first time the creature spawns.

**Cause, most often.** The action set or monster usage set was never registered, so its index
resolved to -1. The usual reason is a **custom `soln_*` id** in `project.mbproj`, which is silently
ignored. See [Registration](/guides/custom_creature_xml/#registration-two-mechanisms-not-interchangeable).

**Cause, also.** A missing `direction="none" turn_direction="none"` reference row for some pace in
the movement table. Every pace needs one.

**How to tell them apart.** If the action set id resolves to -1 at all, it is registration. If it
resolves but the creature dies on a specific movement, it is a missing row.

## Assert about `rglVec3` while loading packages

```
Loading packages $BASE/Modules/<YourModule>/Assets...
Assertion Failed!
...rglBuffer.cpp:899
Expression: (rglMath::nearly_equals(vector->w, 1.0f)) && "Potential read/write miss match for rglVec3"
```

**Cause.** A tool that rewrote a `.tpac` zeroed the 8 bytes at header offset **28 to 35**. Those
bytes are the **table-of-contents size**, and the engine derives `data_start = 36 + toc_size` from
them. Zeroed, the engine believes the data section starts at offset 36, which is where the TOC
begins, so it reads guids and length-prefixed name strings as vectors. The `w` component is not 1.0
and the engine notices.

Measured across 250 shipped tpacs from four modules: `header[28:36]` equals the sum of the item TOC
lengths in 250 of 250, and the first segment's data offset is always exactly `36 + tail`.

**If you write tpac tooling, the gate is one line:**

```
parse the file, re-serialise it with NO modifications, compare byte for byte with the original
```

A dry run that prints plausible numbers proves the script ran, not that the format survived. **Any
field the parser skips is a field the writer will invent, and the fields a parser skips are exactly
the ones nobody has understood yet.**

## Startup assert in `rglIntrusive_ptr.h:151`

```
rglAsset_package_item_texture validate_rdc : Warg_skin_d
Compiled image Warg_skin_d(B8G8R8->DXT1)(2048x2048->2048x2048)
rglAsset_manager::signal_package_item_change - Warg_skin_d
Assertion Failed!  rglIntrusive_ptr.h:151  Expression: px != nullptr
```

**Cause.** A loose `Assets/` definition and a cooked `AssetPackages/` entry both claim one asset
name, **and the loose one's source path is reachable**. The engine really compiles the texture, then
swaps a package item that the cooked pack has already registered under the same name, and
dereferences null.

**The rule.** Dangling is safe. Cooked-only is safe. Both, resolvable, crashes.

A loose definition whose source file is **missing** only produces a warning ("Unable to locate source
file ... to compile") and the game continues. It is tempting to fix that warning by making the path
resolve. Do not: fixing the warning is what causes the crash.

**Fix.** Keep the cooked pack, keep your raw sources for re-baking, and do not ship a loose `Assets/`
tree for that creature. When you do re-bake, do not leave the new loose tpacs beside the old pack,
because that reproduces the same duplicate registration.

## Crash when the creature jumps

**Signature.** Crash in the native monster-usage jump lookup, typically when the creature jumps off
uneven terrain such as a riverbank.

**Cause.** The jump table does not cover the key the engine produced. The parser accepts **nine**
directions and vanilla files only cover the handful vanilla riders generate. A creature driven by
custom AI turns mid-jump and produces the rest.

**Fix.** Write all 45 rows: nine directions across start, loop and end states. A missing key crashes,
an extra row is inert.

Also check `jump_start_action` is typed **`actt_dash`** and not `actt_jump`.

## The rider spawns with no mount

No crash, no error in the log. The rider just appears on foot.

**Cause.** A mesh re-export shipped the geo tpac **mesh-only** and dropped the Skeleton item. The
action set still declares `skeleton="<name>"`, which now resolves to nothing, and agent skeleton
creation returns null.

**Why it is easy to miss.** It degrades gracefully. Every other check passes, because every other
surface really is fine. And a stale baked `AssetPackages/*.tpac` may still contain the old skeleton,
which misleads a byte scan into reporting the skeleton is present, when the engine is loading the
loose `Assets/` copy.

**Fix.** Re-bundle: inject the Skeleton back into the new mesh tpac. Do not build a standalone
skeleton-only tpac, and do not rename the action set's `skeleton=` to a mesh name, which merely
manufactures the null somewhere else.

## The creature slides with its legs frozen

The creature translates across the ground while its legs hold a static pose.

**Cause.** An action was fired on **channel 0** that resolved to `act_none`, usually because the
action name is not registered in any action set, so the index came back as -1.

Channel 0 is the full-body locomotion channel. `SetActionChannel(0, act_none)` does not merely skip
the animation: it freezes the walk cycle while the engine keeps translating the agent.

**Fix.** Check every action name your code fires actually exists in the creature's action set. A
name that "looks right" and was never bound is the common case.

## Invisible in the world, fine in a preview

Often paired with a second symptom that looks unrelated: all colour variants render as the same
colour.

**One cause, both symptoms.** An FBX re-import restored only the material slots the FBX itself
carries. Any material assigned **by hand in the editor** existed only inside the previously compiled
tpac and is now gone.

The colour of a variant lives in its own mesh's material, so every variant falling back to the base
material renders identically. And the missing bindings appear **once per LOD level**, which fits a
creature that renders in a close-up UI preview and vanishes in the world where it draws at a lower
LOD.

!!! warning "A missing material binding does not present as a missing material"
    It presents as the wrong colour, or as nothing at all.

**Fix.** Re-assign in the Kit, and check every LOD. This cannot be repaired by patching the binary:
adding a binding changes the item's metadata length, so it is an insert rather than an overwrite.

## The animation FBX is tiny

A healthy clip export is megabytes and takes seconds. Roughly **0.06 MB in under 0.01 s** means the
bake found no keyframes.

**Cause, on recent Blender.** Renaming edit bones does not rewrite slotted-action fcurve data paths.
Lowercase your bone names and the fcurves still point at the old ones, so there is nothing to bake.

**Byte size is a reliable detector for this.** Check it before you spend time in the Kit.

## The Kit refuses a duplicate name

```
Unable to import skeleton_warg(Skeleton). Item with same name already exists in
Warg_Rig_V5_geo. Asset names are required to be unique within the same module.
```

**This is the protection working.** The Kit enforces per-module name uniqueness, skips the duplicate,
imports the meshes, and lets them bind to the skeleton already present. Nothing is wrong.

Renaming the armature to dodge the message produces a tpac with **no** Skeleton item, which only
looks like success.

## A clip reports zero size and will not save

Also: the Kit refuses to save it, the model viewer draws a scrambled pose, and the file may vanish.

**Cause.** The clip was renamed inside the Modding Kit. The Kit keeps resolving the old name.

**Fix.** Restart the tools. Then rename on disk with the Kit closed instead, and reopen. See
[Compiling in the Kit](/guides/custom_creature_animation/#compiling-in-the-kit).

## Two debugging rules worth more than any single entry above

**A verification that cannot fail is not a verification.** Before trusting a negative result, ask
what the test would show if the hypothesis were true. If the answer is "the same thing", the test is
worthless. Several TAOM hypotheses were "refuted" by tests structurally incapable of detecting the
fault, and two of them were real problems that then cost days.

**Do not build a cause on the half of a correlation you have not checked.** One TAOM creature's
invisibility was confidently attributed to a packaging difference, resting on four creatures that
rendered and two that did not. The failing side had a sample of two, and one of them had never
actually been tested. It rendered fine. The real cause was the missing materials above.
