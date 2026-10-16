# StandingPointWithTeamLimit

!!! info "v1.4.8"

A [StandingPoint](/editor/scripts/standingpoint/) that one team may use. The team is assigned at mission start by the parent gate. The script adds no team field of its own.

Find placed ones in the Scene panel with `s:StandingPointWithTeamLimit`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on a gate stand, usually an editor-only `icon_man`. [CastleGate](/editor/scripts/castlegate/) writes the usable team from **OwningTeam**: attackers or defenders.

## What you set

`Native\Prefabs\props_siege_gates_inner.xml` stores this order on a sally-door stand. `flag_to_raise` stores a shorter list: sheath on, **TranslateUser** on, and empty description and action.

| Panel field | What to do with it |
|---|---|
| **AutoSheathWeapons** | Sheathe on use. The sally stand stores off. The flag stand stores on. |
| **AutoEquipWeaponsOnUseStopped** | Equip on leave. The sally stand stores off. The flag stand does not write this row. |
| **TranslateUser** | Moves the user onto the point. Both store on. |
| **HasRecentlyBeenRechecked** | Runtime flag. The sally stand stores off. The flag stand does not write this row. |
| **NavMeshPrefabName** | Navmesh prefab. The sally stand leaves it empty. |

`mp_siege_map_004` also writes **AutoWieldWeapons** off, own-position off, and interaction distance `0`.

## What it does

After the gate sets the team, agents on that team can use the point. Agents on the other team cannot. A gate owned by Defenders assigns the defender team.

## Where it lives

Managed script. Gate prefabs in `Native\Prefabs\props_siege_gates_inner.xml` and `props_siege_gates_sally.xml`, plus `flag_to_raise` in `props_other.xml`. One multiplayer siege scene stores the script on a baked gate.

## Examples

**Sally door.** `castle_door_sally` / `side` / `icon_man` in `props_siege_gates_inner.xml`. Sheath off, **TranslateUser** on. `european_sally_door_a` has the same stand.

**Flag raise.** `flag_to_raise` / `stand_position` in `props_other.xml`. Sheath on, **TranslateUser** on, description and action empty.

**Baked inner gate.** `Native\SceneObj\mp_siege_map_004`. The gate's **OwningTeam** is Defenders. The team-limit `icon_man` stores sheath off, wield off, own-position off, and interaction distance `0`.
