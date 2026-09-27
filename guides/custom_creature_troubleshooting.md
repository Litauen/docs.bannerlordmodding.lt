# Custom Creature: troubleshooting

Symptom to cause, for custom creatures and mounts. Every entry here is a real failure that was
root-caused, not a guess.

Part of the [Custom Creatures](/guides/custom_creatures/) guide. For crashes with no creature
involved, see [How to report a crash](/guides/how_to_report_a_crash/) and
[Advanced stacktrace analytics](/guides/advanced_stacktrace_analytics_of_crash_reports/).

!!! note "Version"
    Measured on **Bannerlord v1.4.6 to v1.5.3**; each crash offset names the version it was seen on,
    because offsets move with every engine update. Several of these only became crashes in **1.4.6**:
    see [The 1.4.6 rule](/guides/custom_creature_xml/#the-146-rule).

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
| Creature flinches while dealing damage | [An attack bound to a hit-reaction clip](/guides/custom_creature_xml/#the-reskin-trap) |
| Character renders in bind pose in UI tableaux | The action set is valid but binds no clip for that action |
| Crash when a unit walks into water | A standalone action set is missing the dive actions |
| Dedicated server dies at boot, client is fine | A root-level `<action>` element |
| `KeyNotFoundException` at startup | A duplicate `soln_action_sets` row |
| Enormous memory use for one creature | Texture dimensions not divisible by 4 |
| Animation FBX exports at ~0.06 MB | [The bake found no keyframes](#the-animation-fbx-is-tiny) |
| Kit says "Item with same name already exists" | [That is correct behaviour](#the-kit-refuses-a-duplicate-name) |
| Crash on a race's **first swing** or blocked recoil | [A swing clip with no melee-table row](/guides/custom_creature_melee/#the-first-swing-crash) |
| Crash about a second into deployment with a new race | [A race head with no face morph channels](/guides/custom_creature_race/#face-and-hand-morph-channels) |
| A package never appears, log silent | [No RuntimeDataCache entry](/guides/custom_creature_clip_inspector/#what-save-writes-and-the-runtimedatacache) |
| A mount's clip plays in the Kit viewer, never in battle | [Its priority and flags](/guides/custom_creature_clip_inspector/#priority-why-a-clip-plays-in-the-viewer-and-not-in-battle) |
| Body floats or feet skate, limbs right | [Frame 0 is not the rest pose](/guides/custom_creature_animation/#the-export-mapping) |
| Some limbs play another limb's motion | [Bone-track order](/guides/custom_creature_animation/#the-export-mapping) |
| Master renamed `<name>.001`, clip lost its animation | [The take name changed](/guides/custom_creature_animation/#the-export-mapping) |
| `Assigned skeleton animation not found` after a re-import | A junk `<armature>_notused.001` skeleton: delete it, re-import the FBX as an animation, remake the clip |
| Blows pass through parts of the creature | [Default hit capsules](/guides/custom_creature_battle/#three-collision-layers) |
| Broad units stand inside each other | [Foot units are spaced for a human](/guides/custom_creature_battle/#formation-spacing) |
| A big race carries the formation banner | [Banner jobs are open to any humanoid race](/guides/custom_creature_battle/#banners-and-other-jobs-meant-for-humans) |
| A race's fingers never close on the weapon | [No hand-pose morph channels](/guides/custom_creature_race/#face-and-hand-morph-channels) |
| A race character renders as a toddler | [No `<face>`](/guides/custom_creature_race/#what-a-race-is-to-the-engine) |
| An idle plays once in the inventory or a conversation | [A reused clip without that action's flags](/guides/custom_creature_clip_inspector/#flags-at-runtime) |
| Kit warns `Could not set fixed-size(64) string` | [Clip name over 63 characters](/guides/custom_creature_clip_inspector/#clip-names) |
| `Unable to find material` after a re-import | [A material name the module lacks](/guides/custom_creature_skeleton/#check-materials-after-every-fbx-re-import) |
| Null read while a clip plays | [A flag without its clip usage](/guides/custom_creature_clip_inspector/#clip-usages) |
| Crash just after a clip ends | [A Continue to action the set lacks](/guides/custom_creature_clip_inspector/#the-fields-one-by-one) (likely; not reproduced) |
| Kit clip reports `Size in KB = 0` and will not save | [The clip was renamed inside the Kit](#a-clip-reports-zero-size-and-will-not-save) |

## Crash in every mount context at once

**Signature.** Access violation at offset `+0x10`, in `Skeleton.TickAnimations` or
`GetWalkSpeedLimitOfMountable`, seen on v1.4.x; the v1.5.3 offset has not been observed. It fires in the inventory thumbnail, the character tableau **and**
mission deployment, which is the distinguishing feature: three unrelated code paths breaking at once
means the data they share is poisoned, not that any of them is wrong.

**Cause.** A gait clip compiled without the `quad_movement` clip usage, in an action set declared
`movement_system="quadrupedal"`. The set builds a null native gait structure and the first tick
dereferences it.

**Why it survives testing.** The clip compiles without error and plays correctly on a detached,
non-mount agent. It only detonates on quadruped mount machinery.

**Secondary tell.** Resolving an unbound action through the poisoned set returns a runtime-synthesised
garbage name, shaped like `1002467048434979358_0`.

**Fix.** [Add the `quad_movement` clip usage](/guides/custom_creature_animation/#quad_movement-or-the-six-hour-crash).
It is a Clip **usage**, not a Flag. Step points make footsteps; they are not shown to prevent this crash.

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

**Fix.** Restart the tools to clear the Kit's state. That recovers the Kit, not the clip: no clip
renamed inside the Kit is known to be usable afterwards. Rename with the Kit closed, and rename the
clip **item** stored inside its `_anm.tpac`, not only the file name, because the game registers a clip
by that stored name. See [Clip names](/guides/custom_creature_clip_inspector/#clip-names) and
[Compiling in the Kit](/guides/custom_creature_animation/#compiling-in-the-kit).

## Crash on the first swing

**Signature.** On v1.5.3, an access violation at `TaleWorlds.Native.dll+0x6590B9` reading `0x8` to
`0x50` when a race first swings or recoils from a block. The wind-ups play; the release never does.

**Cause.** The clip bound to a release or blocked action has no row in the engine's melee attack table,
or the action is unbound. Only the Kit's "Blends with animation" box gives a clip a row, so a clip
copied from a vanilla template must be keyed again with its own name.

**Fix.** [Melee attack clips](/guides/custom_creature_melee/#three-ways-to-make-a-swing-safe).

## Crash about a second into deployment

**Signature.** On v1.5.3, an access violation at `TaleWorlds.Native.dll+0x57070C` the first time an agent
of a new race is built, in the engine's static face morph ("No morph data found for face mesh").

**Cause.** The race's LOD0 head, eye or mouth has no face morph channels: the Kit writes an empty morph
record, and the engine checks only its pointer.

**Fix.** Give each of them the 101 face morph channels a working race head carries. Zero-offset shape
keys stop the crash, so no reference head is needed; check by counting 101 each on the LOD0 head, eye
and mouth. See [the race page](/guides/custom_creature_race/#face-and-hand-morph-channels).

## A package's items never appear

**Signature.** Meshes, skins or clips from one package never show, and the log is silent.

**Cause.** The game renders a package only when `<module>/RuntimeDataCache/<package GUID>.rdc` exists,
and only the Modding Kit writes it, on save. Animation masters are the exception.

**Fix.** Open the module in the Kit and save. To prove a skip, redefine an existing item name in a
throwaway package: no `Overriding item` log line means it never loaded. See
[the clip inspector](/guides/custom_creature_clip_inspector/#what-save-writes-and-the-runtimedatacache).

## Debugging a native crash

Windows logs the faulting module and offset of a crash to desktop in its Application log, so no symbols
are needed. In PowerShell:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Application'; ProviderName='Application Error'; StartTime=(Get-Date).AddHours(-6)} |
  Where-Object { $_.Message -match "Bannerlord" } |
  ForEach-Object { ($_.Message -split "`n" | Select-Object -First 8) -join "`n"; "---" }
```

**The `Fault offset` is the same for one crash site on one engine build, so compare it across runs:**
the same offset after a fix means the fix failed; a new one is a different crash.

* A crash held by a debugger never reaches that log. Use instruction pointer minus module base, both
  from the same run: the DLL can load at a new address on each launch.
* The Modding Kit's own `TaleWorlds.Native.dll` has different offsets and updates on its own schedule.
  Compare `bin/Win64_Shipping_wEditor/Version.xml` with the game's version before trusting one.
* An assert dialog is a paused state: copy the `rgl_log` and take a full dump (Task Manager, Create dump
  file) before clicking. After Ignore, the logged offset is a secondary site.

Disassemble around the offset: shipping builds keep their assert strings, so functions often name
themselves. A read at a small offset from null means a missing data surface, such as the usage a flag
needs. A hash-map walk ending in a read means a table missing a key: make it total (extra rows are
inert); the key often survives in a register in the dump. TAOM's optional
[native_crash_triage.py](https://github.com/haterade22/TAOM/blob/bannerlord-1.5.x/tools/native_crash_triage.py)
names the function and its strings from an offset or a minidump (pass `--dll`; the default path is
one machine's); any disassembler does the same by hand.

### Known signatures

| Signature | Cause | Evidence |
|---|---|---|
| Access violation at `+0x6590B9` on the first swing | [No melee-table row](/guides/custom_creature_melee/#the-first-swing-crash) | seen in game, v1.5.3 |
| Access violation at `+0x57070C` a second into deployment | [No face morph channels](/guides/custom_creature_race/#face-and-hand-morph-channels) | seen in game, v1.5.3 |
| Access violation at `+0x10` in every mount context | [No `quad_movement`](/guides/custom_creature_troubleshooting/#crash-in-every-mount-context-at-once) | seen in game, v1.4.x |
| Null read at `+0x18`, `+8` or `+0x2C` while a clip plays | [A flag without its usage](/guides/custom_creature_clip_inspector/#clip-usages) | engine code, not reproduced |

### Method

* **Fight a control creature of the same shape first,** a single-creature mount such as a warg.
* **When a rework breaks a working creature, restore its whole backup first:** one file copy can save
  a day.
* **Check the failure signal was absent before your change,** and test in game early: one launch beats
  a long analysis.

## What to search for in the log

The engine log is `C:\ProgramData\Mount and Blade II Bannerlord\logs\rgl_log_<pid>.txt`; read the newest.

| Log text | Meaning |
|---|---|
| `Loading packages` | Which asset trees loaded |
| `Could not find animation:` | A set names an unregistered clip; the slot kept its old value |
| `Trying to use undefined action` | An action missing from `action_types.xml` |
| `could not be found, using default action set!` | A missing `base_set` |
| `Skeleton model could not be found` | A wrong `skeleton=` |
| `does not contain` | A request for an unbound action |
| `Combat parameter not found:` | A clip with no collision window |
| `Sound not found:` | A missing sound code |
| `Could not find face animation record with name:` | A missing facial animation id |
| `Clip usage data couldn't assigned` | A third clip usage, dropped |
| `Unable to register animation clip` | A duplicate clip name |
| `Please set hit_bone_index` | No usable hit bone |
| `get_monster_usage_set_index failed` | A `monster_usage` with no usage set |
| `not found for combat animation blending!` (Kit) | A missing `_balanced` twin |

## Two debugging rules worth more than any single entry above

**A verification that cannot fail is not a verification.** Before trusting a negative result, ask
what the test would show if the hypothesis were true. If the answer is "the same thing", the test is
worthless. Several TAOM hypotheses were "refuted" by tests structurally incapable of detecting the
fault, and two of them were real problems that then cost days.

**Do not build a cause on the half of a correlation you have not checked.** One TAOM creature's
invisibility was confidently attributed to a packaging difference, resting on four creatures that
rendered and two that did not. The failing side had a sample of two, and one of them had never
actually been tested. It rendered fine. The real cause was the missing materials above.
