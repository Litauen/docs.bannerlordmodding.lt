# ShipShieldComponent

!!! info "v1.4.8"

A ship shield that hides while a boarding ramp covers it, and that breaks like a [DestructableComponent](/editor/scripts/destructablecomponent/). It cannot be focused.

Find placed ones in the Scene panel with `s:ShipShieldComponent`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the shield entity. Packed prefabs `shields_holder` and `shields_group` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml` use names such as `shield_lvl1_S`, `shield_lvl2_S`, and `shield_lvl3_N`.

## What you set

`shields_group` stores the destructible rows in this order. Values `0` and `1` are off and on. The class adds no fields of its own.

| Panel field | What to do with it |
|---|---|
| **BattleSide** | `None`, `Defender`, or `Attacker`. The shield stores `None`. |
| **CanBeDestroyedInitially** | Off: hits do nothing until something else enables destruction. The shield stores on (`1`). |
| **DestroyOnAnyHit** | On: the next hit destroys it. The shield stores off (`0`). |
| **DestroyedByStoneOnly** | On: only a siege-stone blow advances the state. The shield stores off (`0`). |
| **DestructionStates** | Comma-separated broken child or prefab names. The shield stores empty. |
| **HeavyHitParticlesTag** | Tag of children that burst on a heavy hit. Empty. |
| **HeavyHitParticlesThreshold** | Damage at which those particles burst. The shield stores `5`. |
| **MaxHitPoint** | Hits needed. The shield stores `100`. |
| **NavMeshPrefabName** | Navmesh prefab. Empty. |
| **ParticleEffectOnDestroy** | Particle id when it is destroyed. Empty. |
| **PassHitOnToParent** | On: this hit is also applied to a destructible parent. Off (`0`). |
| **ReferenceEntityTag** | Child tag whose shape is the intact reference. Empty means this entity. |
| **SoundAndParticleEffectForwardOffset** | Moves the destroy effect along the facing. `0`. |
| **SoundAndParticleEffectHeightOffset** | Raises that effect. `0`. |
| **SoundEffectOnDestroy** | Sound id when it is destroyed. Empty. |

`shields_holder` stores the same values in a different row order.

## What it does

While a boarding ramp is registered against this shield, the entity is hidden. When the last ramp drops, it is shown again. Hits use the destructible rows above.

## Where it lives

Managed script on a destructible. Packed prefabs `shields_holder` and `shields_group` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**South shield.** Prefab `shields_holder`, entity `shield_lvl2_S`. **MaxHitPoint** `100`, **CanBeDestroyedInitially** on, **DestructionStates** empty, **BattleSide** `None`. `shield_lvl1_S` stores the same rows.

**North shield.** Prefab `shields_group`, entity `shield_lvl3_N`. The same values, in the order in the table. Some copies in `shields_holder` (`shield_lvl3_S`, `shield_lvl1_N`) store no variables.
