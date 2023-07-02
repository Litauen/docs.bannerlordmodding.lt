# Persuasion

## Implementation Examples

* LordDefectionCampaignBehavior
* RomanceCampaignBehavior

## PersuasionDifficulty

Used in DefaultPersuasionModel.GetDefaultSuccessChance

    VeryEasy
    Easy
    EasyMedium
    Medium
    MediumHard
    Hard
    VeryHard
    UltraHard
    Impossible


## PersuasionArgumentStrength

    ExtremelyHard = -3
    VeryHard
    Hard
    Normal
    Easy
    VeryEasy
    ExtremelyEasy

??? abstract "Impact on persuasion argument:"

    With PersuasionDifficulty.Medium:

    <center>
    ![](https://imgur.com/cVU0hOH.png)
    </center>


## PersuasionOptionArgs

``` cs
PersuasionOptionArgs(
    SkillObject skill,
    TraitObject trait,
    TraitEffect traitEffect,
    PersuasionArgumentStrength argumentStrength,
    bool givesCriticalSuccess,
    TextObject line,
    Tuple<TraitObject, int>[] traitCorrelation = null,
    bool canBlockOtherOption = false,
    bool canMoveToTheNextReservation = false,
    bool isInitiallyBlocked = false)
```

