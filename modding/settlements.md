# Settlements

- [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_settlements_1_1_settlement.html){target=_blank}
- [Calradia World Map Castles](https://docs.google.com/spreadsheets/d/1aXAwpqAKICjhjr4PGIeXeV1A5GcKsLHDAnNMjX6K1Qs/edit){target=_blank}
- [Calradia World Map Villages](https://docs.google.com/spreadsheets/d/1VqSQAa1xDS3MwrWku55P_roDQS_HWXdLdgzOMhz_O3g/edit){target=_blank}
- [Town Scenes](https://docs.google.com/presentation/d/1GMQqoVQnrmqpTq8xDSfySev2kmBMKiFxNVGRdOazRQs/edit#slide=id.p){target=_blank} by d&h
- [Castle Scenes](https://docs.google.com/presentation/d/1-Wc2IgQPuarkUlFCDsqYNYwqJHyOxlihWFuQOXSG_AQ/edit#slide=id.g2b3b6874ee5_0_45){target=_blank} by d&h
- [Village Scenes](https://docs.google.com/presentation/d/1t6DQuyzKwazqfxxs1_NIQDJk8Pxaj513uT8-T5pVvpg/edit?usp=sharing){target=_blank} by d&h
- [Hideout Scenes](https://docs.google.com/presentation/d/1bhTmkf9ti--o-Jc_qvikNLkVMQw0k5YO-UwtFX_eXaM/edit?usp=drive_link){target=_blank} by d&h

## Gold

    Settlement.SetttlementComponent.Gold

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

## Type of Settlement

    bool value
    .IsTown
    .IsCastle
    .IsVillage
    .IsHideout
    .IsFortification (Town OR Castle)

## Siege

    bool        IsUnderSiege [get]
    SiegeEvent  SiegeEvent [get, set]

## Granary/food

How much food in the granary:

``` cs
Settlement.CurrentSettlement.Town.FoodStocks
```

Max granary amount:

``` cs
Settlement.CurrentSettlement.Town.FoodStocksUpperLimit()
```

## Build projects

Current build project:

``` cs
Building building = Hero.MainHero.CurrentSettlement.Town.CurrentBuilding;
```

Progress percent:

``` cs
float perc = BuildingHelper.GetProgressOfBuilding(building, settlement.Town) * 100;
```

Days to complete:

``` cs
int days = BuildingHelper.GetDaysToComplete(building, settlement.Town);
```

Building tier:

``` cs
int tier = BuildingHelper.GetTierOfBuilding(building.BuildingType, settlement.Town)
```

Buildings in the queue:

``` cs
Queue<Building> settlement.Town.BuildingsInProgress
```

Gold in Reserve:

![](/pics/2409042141.jpg)

``` cs
int settlement.Town.BoostBuildingProcess
```

Construction / "Bricks"

![](/pics/2409042140.jpg)

``` cs
int bricks = settlement.Town.Construction
```

Construction Cost

![](/pics/2409050808.jpg)

```cs
int Building.GetConstructionCost()
```

Building progress - how much is built

![](/pics/2409050819.jpg)

```cs
float Building.BuildingProgress
```

Is building default project?

![](/pics/2409050823.jpg)

```cs
bool building.BuildingType.IsDefaultProject
```


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

To be able to change the owner of the settlement in your mod, you need such files:

- settlements.xml
- settlements.xslt
- CUSTOM_settlements.xml

settlements.xml - full if you are working with the Editor, because Editor requires this file and overwrites it with every map save

settlements.xslt - to delete settlements.xml

CUSTOM_settlements.xml - to apply your changes.


What is interesting/weird - if you change name in the settlements.xml - the name of the settlement is changed in the game. But if you change the owner - owner is not changed. Owner is only changed in CUSTOM_settlements.xml, where 'CUSTOM' can be anything.

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


        <XmlNode>
            <XmlName id="Settlements" path="CUSTOM_settlements"/>
            <IncludedGameTypes>
                <GameType value = "Campaign"/>
                <GameType value = "CampaignStoryMode"/>
                <GameType value = "CustomGame"/>
                <GameType value = "EditorGame"/>
            </IncludedGameTypes>
        </XmlNode>
    ```

??? danger "Never make changes to the CUSTOM_settlements.xml"
    Make changes to the settlemens.xml, then make file copy to the CUSTOM_settlements.xml.
    Otherwise files will be different and Editor uses settlemens.xml and if you change something on the map, changes will go to settlements.xml, it's get deleted with xslt and your CUSTOM file will be used with mismatching map and you will get a crash.

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


### wait_mesh

    Size: 445x805
    SpriteCategory Name="ui_fullbackgrounds"

??? example "Image for the settlement menu and in the Encyclopedia:"
    ![](/pics/2402061353.png)



### background_crop_position

??? note "Not used"
    ``` cs
    SettlementComponent
    public float BackgroundCropPosition { get; protected set; }

    Village
    public override void Deserialize(MBObjectManager objectManager, XmlNode node)
    base.BackgroundCropPosition = float.Parse(node.Attributes["background_crop_position"].Value);

    EmcyclopediaSettlementPageVM
    [DataSourceProperty]
    public double SettlementCropPosition
    {
        get
        {
            return this._settlementCropPosition;
        }
        set
        {
            if (value != this._settlementCropPosition)
            {
                this._settlementCropPosition = value;
                base.OnPropertyChangedWithValue(value, "SettlementCropPosition");
            }
        }
    }

    ```
    no reference in SandBox\GUI\Prefabs\Encyclopedia\EncyclopediaSubPages\EncyclopediaSettlementPage.xml or anywhere else.


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

- v1.2.6b - it is generated by the game, when game starts. Visible in the log.
- v1.2.7 - it is not generated by the game and needs to be manually generated from the Editor.

Can be regenerated with the Editor using [SettlementPositionScript](/editor/settlementpositionscript)

!!! warning "Load time can be long with wrong settlements_distance_cache.bin. [Regenerate to solve it](https://discord.com/channels/411286129317249035/761302555308720148/1282583931119734857)."

<br><br>
