# DisguiseMissionUsePoint

!!! info "v1.4.8"

A use point only the player can use, within 2 meters. The prompt text is **Steal**. The description is **Steal the information**.

Find placed ones in the Scene panel with `s:DisguiseMissionUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the prop the player should focus. The vanilla prop is a barrel. The script is the use point itself, not a machine with a separate pilot child.

## What you set

The class adds no fields. The barrel prefab stores one inherited row.

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh prefab for this entity. The barrel leaves it empty. |

The action text and the description are set in code. They are not rows on the prefab.

## What it does

The player can use it when they are within 2 meters. Other agents cannot. Using it calls the normal use start and then does nothing else in this class: no item is removed and no mission event is fired here.

## Where it lives

Managed script on the usable-object base. The prop is `disguise_mission_interactable_barrel` in `SandBox\Prefabs\sp_props_furniture.xml`.

## Examples

**Information barrel.** Prefab `disguise_mission_interactable_barrel`. Mesh `bd_barrel_a`, physics `bo_bd_barrel_a`. The only stored row is an empty **NavMeshPrefabName**.

**Town drop of that barrel.** `NavalDLC\SceneObj\nord_town_b` places `prefab="disguise_mission_interactable_barrel"` and does not add its own script variables.
