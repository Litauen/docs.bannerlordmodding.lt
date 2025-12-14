# Settlements

- [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_settlements_1_1_settlement.html){target=_blank}
- [Calradia World Map Castles](https://docs.google.com/spreadsheets/d/1aXAwpqAKICjhjr4PGIeXeV1A5GcKsLHDAnNMjX6K1Qs/edit){target=_blank}
- [Calradia World Map Villages](https://docs.google.com/spreadsheets/d/1VqSQAa1xDS3MwrWku55P_roDQS_HWXdLdgzOMhz_O3g/edit){target=_blank}
- [Town Scenes](https://docs.google.com/presentation/d/1GMQqoVQnrmqpTq8xDSfySev2kmBMKiFxNVGRdOazRQs/edit#slide=id.p){target=_blank} by d&h
- [Castle Scenes](https://docs.google.com/presentation/d/1-Wc2IgQPuarkUlFCDsqYNYwqJHyOxlihWFuQOXSG_AQ/edit#slide=id.g2b3b6874ee5_0_45){target=_blank} by d&h
- [Village Scenes](https://docs.google.com/presentation/d/1t6DQuyzKwazqfxxs1_NIQDJk8Pxaj513uT8-T5pVvpg/edit?usp=sharing){target=_blank} by d&h
- [Hideout Scenes](https://docs.google.com/presentation/d/1bhTmkf9ti--o-Jc_qvikNLkVMQw0k5YO-UwtFX_eXaM/edit?usp=drive_link){target=_blank} by d&h

## Find Settlement

```cs
SettlementHelper:
    FindNearestSettlement
    FindNearestHideout
    FindNearestTown
    FindNearestFortification
    FindNearestCastle
    FindNearestVillage
    FindNextSettlementAroundMapPoint
    FindRandomSettlement
    FindRandomHideout
    GetRandomTown
    GetBestSettlementToSpawnAround
```



## Gold

```cs
Settlement.SettlementComponent.Gold
```

## Current Settlement we are at

``` cs
Settlement.CurrentSettlement
```

## Owner

    Settlement.Owner (hero)

## The settlement hero is at currently

    Hero.CurrentSettlement

## Notables

```cs
Settlement.Notables
if (town.Notables.Contains(notable)) return true;
```

??? info "Notables amount by Prosperity"
    ![](/pics/2505080910.png)


## Militia

```cs
float Settlement.Militia

Settlement.Militia += 1
```

## Garrison

`settlement.Town.GarrisonParty`

`SettlementHelper.IsGarrisonStarving`

Create garrison:

```cs
if (settlement.Town.GarrisonParty == null) settlement.AddGarrisonParty();
```

Add troops to garrison:

```cs
settlement.Town.GarrisonParty.MemberRoster.AddToCounts(settlement.Culture.BasicTroop, 1, false, 0, 0, true, -1);
```

## Type of Settlement

    bool value
    .IsTown
    .IsCastle
    .IsVillage
    .IsHideout
    .IsFortification (Town OR Castle)

## [Tournament](/modding/tournaments/)



    Campaign.Current.TournamentManager.GetTournamentGame(town) != null

## Siege

    bool        IsUnderSiege [get]
    SiegeEvent  SiegeEvent [get, set]

### Walls

Fix ~4% of damaged walls:
``` cs
AccessTools.Method(typeof(Town), "RepairWallsOfSettlementDaily").Invoke(settlement.Town, null);
```

Get % of walls hitpoints:

```cs
float wallHealthPercent = settlement.SettlementWallSectionHitPointsRatioList.Average() * 100f;
```

### Last Conquest Time

??? abstract "C#:"
    ```cs
    public static CampaignTime GetLastConquestTime(Settlement settlement)
    {
        for (int i = Campaign.Current.LogEntryHistory.GameActionLogs.Count - 1; i >= 0; i--)
        {
            LogEntry logEntry = Campaign.Current.LogEntryHistory.GameActionLogs[i];
            ChangeSettlementOwnerLogEntry changeLog = logEntry as ChangeSettlementOwnerLogEntry;

            if (changeLog != null && changeLog.Settlement == settlement)
            {
                // Check if this was a true conquest (between different factions)
                IFaction previousFaction = changeLog.PreviousClan?.MapFaction;
                IFaction newFaction = changeLog.NewClan?.MapFaction;

                if (previousFaction != null && newFaction != null && previousFaction != newFaction)
                {
                    return changeLog.GameTime; // This is a true conquest
                }
            }
        }
        return CampaignTime.Never;
    }
    ```
## Granary/food

How much food in the granary:

``` cs
Settlement.CurrentSettlement.Town.FoodStocks
```

Max granary amount:

``` cs
Settlement.CurrentSettlement.Town.FoodStocksUpperLimit()
```

## Build projects / buildings

* [More info here](/modding/buildings)



## Town

Applies to towns AND castles

### TradeBoundVillages

```cs
MBReadOnlyList<Village> Settlement.Town.TradeBoundVillages
```

### Wall Level

```cs
int Settlement.Town.GetWallLevel()
```

### Governor

```cs
Hero Town.Governor

ChangeGovernorAction.RemoveGovernorOf(Hero governor);
ChangeGovernorAction.RemoveGovernorOfIfExists(Town town);
ChangeGovernorAction.Apply(Town fortification, Hero governor);      // assign new
```


## Village

### Village States

```cs
public enum VillageStates
{
    Normal,
    BeingRaided,
    ForcedForVolunteers,
    ForcedForSupplies,
    Looted
}
```

- Looted (same as .IsDeserted)?


### Events

```cs
OnVillageStateChanged(Village village, Village.VillageStates oldState, Village.VillageStates newState, MobileParty raiderParty)
OnVillageBecomeNormal(Village village)
OnVillageBeingRaided(Village village)
OnVillageLooted(Village village)

CampaignEvents.VillageStateChanged.AddNonSerializedListener(this, new Action<Village, Village.VillageStates, Village.VillageStates, MobileParty>(this.OnVillageStateChanged));
CampaignEvents.VillageBeingRaided.AddNonSerializedListener(this, new Action<Village>(this.OnVillageBeingRaided));
CampaignEvents.VillageLooted.AddNonSerializedListener(this, new Action<Village>(this.OnVillageLooted));
```

### GetItemPrice

``` cs
int GetItemPrice(ItemObject item, MobileParty tradingParty = null, bool isSelling = false)
    return this.TradeBound.Town.MarketData.GetPrice(itemRosterElement, tradingParty, isSelling, null);
```


### TradeBound

Returns Town to which the Village is trade bound.

### Levels

![](/pics/2409041125.jpg)

Village has 3 visual levels. Level depends on hearths: 

* Lvl 1 - hearths 1..199
* Lvl 2 - 200-599
* Lvl 3 - 600+

``` cs
settlement.Village.Hearth
```

??? example "Change village hearths method example"
    ``` cs
    public static void ChangeVillageHearths(Settlement settlement, float hearthsChange)
    {
        if (hearthsChange == 0.0 || settlement == null || settlement.Village == null || !settlement.IsVillage || settlement.IsUnderRaid || settlement.IsRaided) return;
        settlement.Village.Hearth += hearthsChange;
    }
    ```


## XML

* [settlements.xml deserialized](/modding/settlements_xml) by Sly

To be able to change the owner of the settlement in your mod, you need such files:

- settlements.xslt
- settlements.xml

settlements.xslt - to delete native settlements.xml

settlements.xml - to add your settlements here


??? info "SubModule.xml should include:"
    ```cs
        <XmlNode>
            <XmlName id="Settlements" path="settlements"/>
            <IncludedGameTypes>
                <GameType value = "Campaign"/>
                <GameType value = "CampaignStoryMode"/>
                <GameType value = "CustomGame"/>
                <GameType value = "EditorGame"/>
            </IncludedGameTypes>
        </XmlNode>
    ```


### Village Production

    VillageType.silk_plant
    VillageType.cattle_farm
    VillageType.silver_mine
    VillageType.iron_mine
    VillageType.lumberjack
    VillageType.wheat_farm
    VillageType.fisherman
    VillageType.europe_horse_ranch
    VillageType.sheep_farm
    VillageType.flax_plant
    VillageType.vineyard
    VillageType.date_farm
    VillageType.olive_trees
    VillageType.swine_farm
    VillageType.clay_mine
    VillageType.trapper
    VillageType.desert_horse_ranch
    VillageType.sturgian_horse_ranch
    VillageType.salt_mine
    VillageType.steppe_horse_ranch
    VillageType.vlandian_horse_ranch
    VillageType.battanian_horse_ranch

### background_mesh

    Size: 100x100
    SpriteCategory Name="ui_fullbackgrounds"

Note: name of the sprite is with the '_t' at the end: PNG file gui_bg_village_baltic_t.png but in the settlements.xml it's gui_bg_village_baltic

??? example "Image for the settlement small icon in the Encyclopedia:"
    ![](/pics/2402061356.png)

!!! warning "From v1.3 you need to add a Brush for each icon."


### wait_mesh

    Size: 445x805
    SpriteCategory Name="ui_fullbackgrounds"

??? example "Image for the settlement menu and in the Encyclopedia:"
    ![](/pics/2402061353.png)




### castle_background_mesh

??? note "Not used"

    ``` cs
    Village
    public string CastleBackgroundMeshName { get; protected set; }

    base.CastleBackgroundMeshName = node.Attributes["castle_background_mesh"].Value;
    ```
    and CastleBackgroundMeshName is not used anywhere else.


## Settlements Distance Cache

!!! quote "If you add new settlements, or relocate existing ones, you will need to recalculate settlements_distance_cache.bin which the game uses to optimise AI path finding between different settlements."

!!! info "From my observation: when adding new settlements it's not necessary to recalculate it, game works as it is. It is necessary to recalculate at the end (when all settlements are added) for performance."

!!! danger "The game crashes if settlements.xml and settlements_distance_cache.bin are incompatible."

</p>

    settlements_distance_cache.bin

Editor does not generate it by default on map save.

If settlements_distance_cache.bin is missing:

- v1.3+ - game regenerates it automatically if missing

Can be regenerated with the Editor using [SettlementPositionScript](/editor/settlementpositionscript)

!!! warning "Load time can be long with wrong settlements_distance_cache.bin. [Regenerate to solve it](https://discord.com/channels/411286129317249035/761302555308720148/1282583931119734857)."

<br><br>
