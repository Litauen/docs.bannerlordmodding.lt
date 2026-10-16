# HideoutBossFightBehavior

!!! info "v1.4.8"

Marks the hideout boss-fight ground and sizes the two rings the cinematic uses for the player, the boss, and their crews.

Find placed ones with `s:HideoutBossFightBehavior`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on one entity tagged `hideout_boss_fight`. The hideout cinematic finds that tag and reads the three distances. Face the entity the way the two sides should approach: the boss side is in front, the player side is behind.

**Show Preview** spawns editor-only figures. Those previews look for child entities tagged `initial_frame` and `target_frame`.

## What you set

| Panel field | What to do with it |
|---|---|
| **InnerRadius** | Radius of the player and the boss. Script default `2.5`. The prefab stores `2`. |
| **OuterRadius** | Radius of the ally and bandit lines. Prefab and default: `6`. |
| **WalkDistance** | How far each side starts before stepping in. Script default `3`. The prefab stores `4`. |
| **ShowPreview** | Editor figures for the boss, the player, allies, and bandits. Prefab: off. Several hideout scenes store it on. |

## What it does

The player and the boss are placed on the inner ring. Allies and bandits are placed on the outer ring, up to 10 a side. **WalkDistance** is how far behind those spots the walk-in starts. The camera is kept inside a box about `10` wide and `5` high, reaching **OuterRadius** plus **WalkDistance** in front and behind.

The preview uses `hideout_boss_fight_preview_boss`, `hideout_boss_fight_preview_player`, `hideout_boss_fight_preview_ally`, `hideout_boss_fight_preview_bandit`, and `hideout_boss_fight_camera_preview`.

## Where it lives

`Modules\SandBox\Prefabs\sp_hideout_prefabs.xml`, entity `hideout_boss_fight`. The prefab is visible only while editing and carries the tag `hideout_boss_fight`.

## Examples

**Prefab defaults.** `hideout_boss_fight`: inner `2`, outer `6`, walk `4`, preview off.

**Tighter inner ring.** `SandBox\SceneObj\bandit_forest` and `SandBox\SceneObj\bandit_forest_sv` place `hideout_boss_fight` with inner `1.5`, outer `6.5`, preview on. Walk stays the prefab `4`.

**Wider camp.** `SandBox\SceneObj\forest_hideout_003` stores inner `2.110`, outer `7.950`, walk `3.990`, preview on.

**Preview only.** `SandBox\SceneObj\desert_hideout_004`, `SandBox\SceneObj\sea_bandit_c`, `SandBox\SceneObj\desert_hideout_002_sv`, and `SandBox\SceneObj\sea_bandit_a_sv` place the prefab and only turn **ShowPreview** on. Radii stay `2` and `6`, walk stays `4`.
