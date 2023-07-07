# Persuasion

## Implementation Examples

### LordDefectionCampaignBehavior

??? abstract "Structure of LordDefectionCampaignBehavior"

    [Fullsize Link](https://i.imgur.com/j5Sm0Zs.png){target=_blank}

    ![](https://imgur.com/j5Sm0Zs.png)


### RomanceCampaignBehavior

### [Trade Agent mod](https://www.nexusmods.com/mountandblade2bannerlord/mods/5618){target=_blank}

[Link](https://github.com/Litauen/LT_TradeAgent/blob/master/LTTABarter.cs){target=_blank}

??? abstract "Structure"

    ![](https://imgur.com/OjHOkDe.png)

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

??? danger "Make sure you are not going to -4, like ExtremelyHard - 1, that way it overflows to Normal"

    Possible function to clamp values:

    ``` cs
    PersuasionArgumentStrength GetPersuasionArgumentStrength(int value)
    {
        PersuasionArgumentStrength persuasionArgumentStrength = PersuasionArgumentStrength.Normal;

        if (value < -2) return PersuasionArgumentStrength.ExtremelyHard;
        if (value == -2) return PersuasionArgumentStrength.VeryHard;
        if (value == -1) return PersuasionArgumentStrength.Hard;
        if (value == 1) return PersuasionArgumentStrength.Easy;
        if (value == 2) return PersuasionArgumentStrength.VeryEasy;
        if (value > 2) return PersuasionArgumentStrength.ExtremelyEasy;

        return persuasionArgumentStrength;
    }
    ```


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

