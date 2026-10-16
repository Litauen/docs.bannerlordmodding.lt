# AnimalSpawnSettings

!!! info "v1.4.8"

Stops the animal spawned from this entity from wandering. The entity tag decides which animal appears. **DisableWandering** is on by default.

Find placed ones in the Scene panel with `s:AnimalSpawnSettings`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the spawn point, not to the animal. The point needs one of the tags the town and village spawn reads:

| Tag | What spawns |
|---|---|
| `sp_horse` | A horse. The second tag is the horse item id (`war_horse`, `aserai_horse`, `khuzait_horse`, …). |
| `sp_sheep` | A sheep |
| `sp_cow` | A cow |
| `sp_goose` | A goose |
| `sp_chicken` | A chicken |
| `sp_hog` | A hog |

Vanilla points are editor-only and use the yellow stand arrow.

## What you set

| Panel field | What to do with it |
|---|---|
| **DisableWandering** | On: the spawned animal loses the wander flag. Off: the script is on the point, and the animal keeps that flag. The default is on. |

A prefab that stores the script with no variable still uses that default.

## What the player gets

The animal stands where the point is and does not walk off. Town and village missions spawn horses, sheep, cows, geese, chickens, and hogs from those tags, then apply this check. A stealth cover cow applies the same check to the patrol child. The training field applies it to the entity tagged `spawner_melee_npc`.

## Where it lives

Managed script. The spawn-point prefabs are in `Modules\Native\Prefabs\editor_spawnpoints.xml`. The only scene that stores the field is `SandBoxCore\SceneObj\empire_town_l`: two `sp_horse_war_horse` points, both with **DisableWandering** on.

``` xml
<script name="AnimalSpawnSettings">
    <variables>
        <variable name="DisableWandering" value="true"/>
    </variables>
</script>
```

## Examples

Each of these is an entity in `editor_spawnpoints.xml`. The script is present and the variable is left off, so wandering stays disabled.

**Empire war horse.** `sp_horse_war_horse`, tags `sp_horse` and `empire_horse`.

**Aserai war horse.** `sp_horse_desert_war_horse`, tags `sp_horse` and `aserai_horse`.

**Khuzait charger.** `sp_horse_steppe_charger`, tags `sp_horse` and `khuzait_horse`.

**Vlandian charger.** `sp_horse_charger`, tags `sp_horse` and `vlandia_horse`.

**Battanian horse.** `sp_horse_battania`, tags `sp_horse` and `battania_horse`.

**Sturgian hunter.** `sp_horse_hunter`, tags `sp_horse` and `sturgia_horse`.

**Pen animals.** `sp_sheep`, `sp_cow`, `sp_goose`, `sp_chicken`, and `sp_hog`. Each has only its own spawn tag.

**Town horses that store the field.** `empire_town_l` places two `sp_horse_war_horse` points, tags `sp_horse` and `war_horse`, and writes **DisableWandering** `true`. Those points also show on the siege level.
