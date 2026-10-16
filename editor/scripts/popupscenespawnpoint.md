# PopupSceneSpawnPoint

!!! info "v1.4.8"

Marks where a notification character stands, which action they play, and what they hold. The notification fills the body. This script supplies the pose.

Find placed ones in the Scene panel with `s:PopupSceneSpawnPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an editor-only entity with a human skeleton and the stand arrow. Tag it `spawnpoint_player_1`, `spawnpoint_player_2`, and so on. The notification walks those tags in order and skips a number that has no entity.

The ready-made prefab is `cutscene_agent_template` in `Modules\SandBox\Prefabs\cutscene_props.xml`.

## What you set

The template stores this order. Scenes that need the extra rows write those as well.

| Panel field | What to do with it |
|---|---|
| **InitialAction** | Action when the notification opens. Empty means no action. |
| **PositiveAction** | Action when the player continues. Empty keeps the initial action. |
| **NegativeAction** | Action for the negative state. Empty keeps the initial action. The notification does not call that state. |
| **InitialFaceAnimCode** | Face clip for the open. Loops. |
| **PositiveFaceAnimCode** | Face clip when the player continues. Loops. |
| **NegativeFaceAnimCode** | Face clip for the negative state. Loops. |
| **RightHandWieldedItem** | Item id in the first weapon slot, wielded in the right hand. |
| **PrefabItem** | Prefab parented to a bone on the character. Empty skips it. |
| **PrefabBone** | Bone for that prefab. The template stores `ItemR`. |
| **LeftHandWieldedItem** | Item id in the second weapon slot, wielded in the left hand. The template leaves this row off. |
| **BannerTagToUseForAddedPrefab** | If this matches a banner slot such as `banner_1`, the banner texture is applied to the added prefab. |
| **StartWithRandomProgress** | On: the initial action starts at a random point in the first half of the clip. Off starts at `0`. |
| **AttachedPrefabOffset** | Local shift of the added prefab. Default zero. |

Some scenes also save `UseHorse`. This script does not read it. The notification decides the horse.

The template, from `cutscene_props.xml`:

``` xml
<script name="PopupSceneSpawnPoint">
    <variables>
        <variable name="InitialAction" value="mainstory_cutscene_conspiracy_walk_1"/>
        <variable name="PositiveAction" value="mainstory_cutscene_conspiracy_talk_1"/>
        <variable name="NegativeAction" value=""/>
        <variable name="InitialFaceAnimCode" value=""/>
        <variable name="PositiveFaceAnimCode" value=""/>
        <variable name="NegativeFaceAnimCode" value=""/>
        <variable name="RightHandWieldedItem" value=""/>
        <variable name="PrefabItem" value=""/>
        <variable name="PrefabBone" value="ItemR"/>
    </variables>
</script>
```

## What it does

On open, the character and their mount play **InitialAction**, and the face uses **InitialFaceAnimCode**. Weapons are written into the equipment and wielded. **PrefabItem** is attached to **PrefabBone**.

When the player continues, the notification calls the positive state: **PositiveAction** and **PositiveFaceAnimCode**. Entities tagged with this spawn point’s **name** get the same initial or positive call, which is how a [PopupSceneSequence](/editor/scripts/popupscenesequence/) stays in step with the character.

## Where it lives

Managed script. The template is `Modules\SandBox\Prefabs\cutscene_props.xml`. Notification scenes under `SandBox\SceneObj\scn_*` store the overrides.

## Examples

**Conspiracy walker.** Prefab `cutscene_agent_template`. Tags `npc_wait` and `spawnpoint_player_4`. Initial `mainstory_cutscene_conspiracy_walk_1`, positive `mainstory_cutscene_conspiracy_talk_1`.

**Mother with the baby.** `SandBox\SceneObj\scn_born_baby`. Entity `sp_maid`, tag `spawnpoint_player_2`. Initial `act_cutscene_born_baby_mom`. **PrefabItem** `baby_right` on `ItemR`.

**Priest at the wedding.** `SandBox\SceneObj\scn_cutscene_wedding`. Entity `sp_wedding_priest`, tag `spawnpoint_player_3`. Initial `act_cutscene_wedding_priest`. All three face rows are `talking_normal`.

**Executioner with the axe.** `SandBox\SceneObj\scn_execution_notification`. Entity `sp_player_executioner`, tag `spawnpoint_player_2`. Positive action `act_cutscene_executioner_action`. **RightHandWieldedItem** `execution_axe`.

**A lord who swears, sword and shield.** `SandBox\SceneObj\scn_cutscene_declare_dragon_banner`. Entity `pledge_pose_14`, tag `spawnpoint_player_1`. Left hand `battania_large_shield_a`, right hand `battania_sword_4_t4`. Face `declare_dragon_banner_face_01`.

**Heir holding a practice sword.** `SandBox\SceneObj\scn_cutscene_heir_coming_of_age`. Entity `heir_character_stage_2`, tag `spawnpoint_player_3`. **PrefabItem** `training_sword` on `ItemR`.

**Crowd that is already mid-cheer.** `SandBox\SceneObj\scn_become_king_notification`. Prefab `cutscene_agent_template`, tag `spawnpoint_player_14`. Initial `act_main_story_become_king_female_crowd_5`. **StartWithRandomProgress** on. The point tagged `spawnpoint_player_12` does the same with `act_main_story_become_king_crowd_07`.

**Sister at the hatch.** `NavalDLC\SceneObj\cutscene_saving_sister`. Prefab `cutscene_agent_template`, tag `spawnpoint_player_3`. The scene replaces the action with `act_cutscene_sister_save_door` and clears **PositiveAction**.
