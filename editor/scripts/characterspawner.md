# CharacterSpawner

!!! info "v1.4.8"

Editor preview of one character, and optionally their horse, posed on this entity. **Enabled** has to be on before a visual appears.

Find placed ones in the Scene panel with `s:CharacterSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity where the figure should stand. Conversation scenes drop the infantry-spawn prefab and leave **Enabled** off, so the editor stays empty until you turn it on.

## What you set

Prefab `player_infantry_spawn` in `Native\Prefabs\editor_spawnpoints.xml` stores the rows in this order.

| Panel field | What to do with it |
|---|---|
| **Enabled** | On: spawn the visual. Off: remove it. Default off. |
| **PoseAction** | Action name on the person. Default `act_walk_idle_unarmed`. |
| **LordName** | Character id. Default `main_hero_for_perf`. Equipment, gender, and banner come from this character. |
| **ActionSetSuffix** | Suffix on the action set. Default `_facegen`. Spawn prefabs often use `_villain` or `_warrior`. |
| **PoseActionForHorse** | Action name on the horse. Default `horse_stand_3`. |
| **BodyPropertiesString** | Face and body string used for the visual. The infantry-spawn prefab stores a face key. |
| **IsWeaponWielded** | Wield a weapon that matches the character's formation (melee or ranged). |
| **HasMount** | Spawn the horse from the character's horse slot. If that slot is empty, this turns itself off. |
| **WieldOffHand** | Also wield a shield, or the ammo that matches a ranged weapon. Default on. |
| **AnimationProgress** | Freeze the person at this point in the action, from `0` to `1`. |
| **HorseAnimationProgress** | Same freeze for the horse. |
| **Active** | Shows or hides the entity, the person, and the horse. |

## What it does

With **Enabled** on, the editor builds a character visual from **LordName** and **BodyPropertiesString**, plays **PoseAction**, and freezes it at **AnimationProgress**. **HasMount** adds a horse child playing **PoseActionForHorse**. The visual is editor-only. A mission does not spawn an agent from this script.

## Where it lives

Managed script. Ready-made entities are in `Native\Prefabs\editor_spawnpoints.xml` (`player_infantry_spawn`), `Native\Prefabs\props_other.xml` (`character_portrait_pose`), and `SandBox\Prefabs\sp_editor_spawnpoints.xml` (`arena_spectator`).

## Examples

**Camel rider, preview left on.** `Native\SceneObj\scn_soldier`. Entity `camel_troop_cavalry_lance_pose`. **Enabled** is on. **LordName** is `jawwal_tier_2`, **PoseAction** is `act_camel_tableau_pose_rider_polearm`, **PoseActionForHorse** is `act_camel_tableau_pose`, **ActionSetSuffix** is `_warrior`.

**Conversation pair, preview left off.** `SandBox\SceneObj\conversation_scene_forest`. `player_infantry_spawn` keeps the prefab character `prison_guard_battania` and pose `act_bandit_thumbnail_pose`, **Enabled** off. `opponent_infantry_spawn` sets **LordName** to `commander_6`, **PoseAction** to `act_tableau_pose_7`, and **ActionSetSuffix** to `_facegen`.

**Sea conversation spawn.** `NavalDLC\SceneObj\conversation_scene_sea`. Same `player_infantry_spawn` prefab, **Enabled** off.

**Arena crowd pose.** Prefab `arena_spectator` stores **PoseAction** `act_arena_spectator`, **LordName** `main_hero`, **ActionSetSuffix** `_warrior`, weapon off. It does not store **Enabled**.

**Portrait prop.** Prefab `character_portrait_pose` leaves **Enabled** off, **LordName** `main_hero_for_perf`, **PoseAction** `act_walk_idle_unarmed`, **ActionSetSuffix** `_warrior`.
