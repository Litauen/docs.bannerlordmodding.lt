# HideoutSpawnPointGroup

!!! info "v1.4.8"

A set of four hideout defender spawn markers for one fight phase. The phase number decides which wave can use the set.

Find placed ones with `s:HideoutSpawnPointGroup`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on a parent whose children are the spawn markers. The script reads **Side** (`Defender` or `Attacker`) and looks for a child tagged `{side}_{formation}`:

- `defender_infantry`
- `defender_ranged`
- `defender_cavalry`
- `defender_horsearcher`

The attacker names are the same with `attacker_`. The markers are editor-only arrows. After the hideout reads the frames, it deletes the group.

## What you set

| Panel field | What to do with it |
|---|---|
| **Side** | `Defender` or `Attacker`. The prefab stores `Defender`. The name is lowercased and prefixed onto the child tags. |
| **PhaseNumber** | Wave index, starting at `1`. The prefab stores `1`. The mission keeps phase 1 and a random later phase, and drops the rest. |

The scene copies also store **NavMeshPrefabName**, left empty.

## What it does

On start, each of the four formation tags is matched to one child. A missing child yields an empty frame and that formation is not spawned. The hideout mission files the groups by **PhaseNumber** (phase 1 is index 0), copies the frames it keeps, and deletes every group. The shipped scenes only store phase 1.

## Where it lives

`Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml`, entity `sp_hideout_group_defender`.

`sp_hideout_group_attacker` in the same file has `attacker_infantry` and `attacker_archer` children and does not have this script.

## Examples

**Defender phase 1.** `sp_hideout_group_defender`: **Side** `Defender`, **PhaseNumber** `1`. Children `sp_defender_infantry` (`defender_infantry`), `sp_defender_archer` (`defender_archer` and `defender_ranged`), `sp_defender_cavalry`, and `sp_defender_horsearcher`. The ranged slot is the `defender_ranged` tag.

**Baked into a hideout.** `SandBox\SceneObj\bandit_forest_sv`, `SandBox\SceneObj\desert_hideout_002_sv`, and `SandBox\SceneObj\sea_bandit_a_sv` each store one `sp_hideout_group_defender` with **Side** `Defender`, **PhaseNumber** `1`, and an empty navmesh name. No official scene stores a second phase.
