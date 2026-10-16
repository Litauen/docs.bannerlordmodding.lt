# DestructableComponent

!!! info "v1.4.8"

Hit points for an entity. At the next destruction step the intact mesh is swapped for the next broken child or broken prefab, and debris can be pushed.

Find placed ones in the Scene panel with `s:DestructableComponent`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the object that should break. Tag the intact child `operational`. **DestructionStates** names the broken versions, in order, separated by commas. Each name is either a child of this entity or a prefab that can be spawned.

## What you set

Castle merlons in `Native\Prefabs\castle_aserai.xml` store the rows in this order.

| Panel field | What to do with it |
|---|---|
| **DestructionStates** | Comma-separated child names or prefab names. The merlon stores `castle_wall_roof_aserai`. |
| **DestroyedByStoneOnly** | On: only a siege-stone blow advances the state. Merlons leave this on. |
| **CanBeDestroyedInitially** | Off: hits do nothing until something else enables destruction. |
| **MaxHitPoint** | Hits needed. Default `100`. Merlons store `1`. |
| **DestroyOnAnyHit** | On: the next hit destroys it, ignoring hit points. |
| **PassHitOnToParent** | On: this hit is also applied to a destructible parent. |
| **ReferenceEntityTag** | Child tag whose shape is the intact reference. Merlons use `reference`. Empty means this entity. |
| **HeavyHitParticlesTag** | Tag of children that burst on a heavy hit. |
| **HeavyHitParticlesThreshold** | Damage at which those particles burst. Default `5`. |
| **ParticleEffectOnDestroy** | Particle id played when it is destroyed. |
| **SoundEffectOnDestroy** | Sound id played when it is destroyed. |
| **SoundAndParticleEffectHeightOffset** | Raises that effect. |
| **SoundAndParticleEffectForwardOffset** | Moves that effect along the facing. |
| **BattleSide** | `None`, `Defender`, or `Attacker`. Gates force `Defender` on their own component at mission start. |
| **NavMeshPrefabName** | Navmesh prefab for this entity. Vanilla leaves it empty. |

## What it does

Hits subtract from **MaxHitPoint**. **DestroyOnAnyHit** or a drained bar advances the destruction index, hides the piece tagged `operational`, and shows the next name in **DestructionStates**. Dynamic children of that broken piece are turned loose and pushed. **DestroyedByStoneOnly** ignores blows that are not siege stones.

A castle gate on the same entity listens for the destroyed state and opens from it.

## Where it lives

Managed script. Castle pieces are in `Native\Prefabs\castle_*.xml` and `NavalDLC\Prefabs\castle_nord_l1.xml` (also `l2` and `l3`). Ship weapons are in `NavalDLC\Prefabs\ship_machines.xml`.

## Examples

**Merlon that only a stone breaks.** `Native\Prefabs\castle_aserai.xml`, entity `merlon_1`. **DestroyedByStoneOnly** on, **MaxHitPoint** `1`, **DestructionStates** `castle_wall_roof_aserai`, **ReferenceEntityTag** `reference`, **BattleSide** `None`.

**Nord merlon in a shipyard.** `NavalDLC\SceneObj\nord_shipyard`. A wall merlon uses **DestructionStates** `debris_vlandia_l1`, stone-only, **MaxHitPoint** `1`, reference tag `reference`.

**Mangonel body.** `NavalDLC\Prefabs\ship_machines.xml`, entity `mangonel_b_body`. **DestructionStates** `destroyed`, **CanBeDestroyedInitially** off, **MaxHitPoint** `900`, **BattleSide** `Defender`, destroy sound `event:/mission/siege/mangonel/break`.

**Training board that any hit breaks.** `StoryMode\SceneObj\training_field_2`. Entity `target_board_colored`, tag `mounted_bow`. **DestructionStates** `target_board_colored_dark_1`, **DestroyOnAnyHit** on, **MaxHitPoint** `1`.

**Hanging pot.** Same scene, entity `archery_target_pot`, tag `mounted_melee`. **DestructionStates** `hanging_pot_destroyed`, **CanBeDestroyedInitially** off, **MaxHitPoint** `0.100`, **DestroyOnAnyHit** on.

**Javelin pot.** `StoryMode\SceneObj\training_field`. Tag `ranged_javelin`. **DestructionStates** `pot_archery_destroyed`, **DestroyOnAnyHit** off, **MaxHitPoint** `0.100`, **CanBeDestroyedInitially** off.
