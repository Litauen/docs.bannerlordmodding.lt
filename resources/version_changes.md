# Version changes

## 1.2.9 -> 1.2.10

### TextObject.Empty field changed to property

Crash:

    Type: System.MissingFieldException
    Message: Field not found: 'TaleWorlds.Localization.TextObject.Empty'.

A simple recompile should fix the crash.

If you are using reflection to reference TextObject.Empty, you need to change AccessTools.Field to AccessTools.Property, or GetField to GetProperty. [[order_without_power](https://discord.com/channels/411286129317249035/677511186295685150/1250149875614744647)]

## 1.1.6 -> 1.2.7

### Settlement Prosperity

Moved from settlement to settlement.town

### MultiSelectionInquiryData

New field: int minSelectableOptionCount

### MenuOption order in the menu

``` cs
AddGameMenuOption(string menuId, string optionId, string optionText, GameMenuOption.OnConditionDelegate condition, GameMenuOption.OnConsequenceDelegate consequence, bool isLeave = false, int index = -1, bool isRepeatable = false, object relatedObject = null)
```

