# Quests

## Tutorials

* [Creating a quest](https://forums.taleworlds.com/index.php?threads/creating-a-quest.415596/){target=_blank}
* [Additional Quests](https://www.nexusmods.com/mountandblade2bannerlord/mods/3066) - demo mod for quests

## Tools

* [Quest Debugger](https://www.nexusmods.com/mountandblade2bannerlord/mods/8132) - Modding tool that allows to start quest from native console and adds the Quest Debug Console that shows active quests instances properties and fields and their values.


## Chars for quests/etc

??? quote "Eagle: you'll need those characters for quests / encounters and character creation (don't delete from XML)"
    - spc_e3_character_1
    - spc_e3_character_2
    - spc_e3_character_3
    - spc_e3_character_4
    - spc_e3_character_5
    - spc_e3_character_6
    - poacher
    - company_of_trouble_character
    - hardy_contender_easy
    - hardy_contender_normal
    - hardy_contender_hard
    - hardy_contender_very_hard
    - dignified_contender_easy
    - dignified_contender_normal
    - dignified_contender_hard
    - dignified_contender_very_hard
    - confident_contender_easy
    - confident_contender_normal
    - confident_contender_hard
    - confident_contender_very_hard
    - bold_contender_easy
    - bold_contender_normal
    - bold_contender_hard
    - bold_contender_very_hard
    - hardy_contender
    - dignified_contender
    - confident_contender
    - bold_contender
    - betting_fraud_thug_male
    - betting_fraud_thug_female
    - borrowed_troop
    - veteran_borrowed_troop
    - mounted_pillager
    - mounted_ransacker
    - cutscene_midwife
    - cutscene_monk
    - sp_hermit


## Check if quest is active

``` cs
Campaign.Current.QuestManager.IsThereActiveQuestWithType(typeof(YOURQuest));
```