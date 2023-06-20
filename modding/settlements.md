# Settlements

[API](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_settlements_1_1_settlement.html){target=_blank}

## Current Settlement we are at

``` cs
Settlement.CurrentSettlement
```

## The settlement hero is at currently

    Hero.CurrentSettlement

## Notables

    Settlement.Notables
    if (town.Notables.Contains(notable)) return true;

## Type of Settlement

    bool value
    .IsTown
    .IsCastle
    .IsVillage
    .IsHideout
    .IsFortification (Town OR Castle)

## Granary/food

How much food in the granary:

``` cs
Settlement.CurrentSettlement.Town.FoodStocks
```

Max granary amount:

``` cs
Settlement.CurrentSettlement.Town.FoodStocksUpperLimit()
```

## Town

### TradeBoundVillages

    Settlement.Town.TradeBoundVillages

## Village

### GetItemPrice

``` cs
int GetItemPrice(ItemObject item, MobileParty tradingParty = null, bool isSelling = false)
    return this.TradeBound.Town.MarketData.GetPrice(itemRosterElement, tradingParty, isSelling, null);
```

### VillageState

- Normal
- BeingRaided
- ForcedForVolunteers
- ForcedForSupplies
- Looted (same as .IsDeserted)

### TradeBound

Returns Town to which the Village is trade bound.



