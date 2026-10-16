# SkeletalMeshEditer

!!! info "v1.4.8"

A posed corpse on a human skeleton. The editor spelling is `SkeletalMeshEditer`. The bone pose is stored on the skeleton. The script row vanilla files keep is **Ragdoll Enabled**.

Find placed ones in the Scene panel with `s:SkeletalMeshEditer`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity that already has a skeleton. The vanilla copies use `human_skeleton` with `body_male_a`, `head_male_a`, `hands_male_a`, and `feet_male_a`. `corpse_editer` also stores a bone pose and helper children such as `pelvis_helper`.

## What you set

Native panel. All three copies store the same single row.

| Panel field | What to do with it |
|---|---|
| **Ragdoll Enabled** | Ragdoll on this skeleton. The three prefabs store off. |

## What it does

The entity keeps the skeleton and the bone frames saved on it. With **Ragdoll Enabled** off, the pose stays as authored.

## Where it lives

Native script. The copies are in `Native\Prefabs\props_other.xml`. No scene file stores the script.

## Examples

**Attacker corpse.** `corpse_attacker`. Tag `coprse_attacker`. Skeleton `human_skeleton`. **Ragdoll Enabled** off. No bone frames and no helper children.

**Posed corpse.** `corpse_editer`. Same skeleton and body meshes, with bone frames and a `pelvis_helper` child. **Ragdoll Enabled** off.

**Dead body.** `dead_body` has a child also named `corpse_editer`, with its own bone frames. **Ragdoll Enabled** off. The parent is tagged `dead_body` and `attacker`.
