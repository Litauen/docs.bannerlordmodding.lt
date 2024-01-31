# Version changes

## 1.1.6 -> 1.2.7

### Settlement Prosperity

Moved from settlement to settlement.town

### MultiSelectionInquiryData

New field: int minSelectableOptionCount

### MenuOption order in the menu

``` cs
AddGameMenuOption(string menuId, string optionId, string optionText, GameMenuOption.OnConditionDelegate condition, GameMenuOption.OnConsequenceDelegate consequence, bool isLeave = false, int index = -1, bool isRepeatable = false, object relatedObject = null)
```

