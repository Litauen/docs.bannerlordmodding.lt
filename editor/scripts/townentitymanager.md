# Town Entity Manager

!!! info "v1.4.8"

Native panel. The color override on a campaign-map settlement icon.

Find placed ones with `s:Town Entity Manager`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the settlement icon entity, inside a `campaign_icon_capsule_*` parent. The icon is tagged `village`, `castle`, or `town`. The SandBox main map uses it on `castle_village_S5_2`. The rain test scene uses it on `town_V5`.

## What you set

Native panel. Both opened entities store this order.

| Panel field | What to do with it |
|---|---|
| **Override Factor Color** | On: use **Factor Color** on this icon. The main-map village stores off. `town_V5` in the rain scene stores on. |
| **Factor Color** | Color, four numbers. Both opened entities store `1, 1, 1, 1`. |

## What it does

The icon keeps the settlement mesh. **Override Factor Color** decides whether **Factor Color** replaces the mesh factor. White with the override off leaves the prefab color alone.

## Where it lives

Native script. Placed in the main-map scenes and in `Native\SceneObj\rain`.

## Examples

**Sturgia village icon.** `SandBox\SceneObj\Main_map`. Entity `castle_village_S5_2`, tagged `village` and `castle`, under `campaign_icon_capsule_80`. Override off, color white.

**Rain-scene town.** `Native\SceneObj\rain`. Entity `town_V5`, tagged `town`. Override on, color white.

`NavalDLC\SceneObj\Main_map` stores the same script.
