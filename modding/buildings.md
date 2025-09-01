# Buildings

* [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_settlements_1_1_buildings_1_1_building.html)

## Current build project

``` cs
Building building = Hero.MainHero.CurrentSettlement.Town.CurrentBuilding;
```

## Progress percent

``` cs
float perc = BuildingHelper.GetProgressOfBuilding(building, settlement.Town) * 100;
```

## Days to complete

``` cs
int days = BuildingHelper.GetDaysToComplete(building, settlement.Town);
```

## Building level/tier

``` cs
int tier = BuildingHelper.GetTierOfBuilding(building.BuildingType, settlement.Town)

int building.CurrentLevel
```

## Current Construction Cost

![](/pics/2409050808.jpg)

```cs
int Building.GetConstructionCost()
```


## Construction cost at level

```cs
building.BuildingType.GetProductionCost(level)
```


## Building progress

How much is built?

![](/pics/2409050819.jpg)

```cs
float Building.BuildingProgress
```

## Is default project?

![](/pics/2409050823.jpg)

```cs
bool building.BuildingType.IsDefaultProject
```

