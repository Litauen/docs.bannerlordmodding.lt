# MapEvent

* [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_map_events_1_1_map_event.html)

## BattleTypes

```cs
BattleTypes     EventType [get]

BattleTypes {
  None ,
  FieldBattle ,
  Raid ,
  IsForcingVolunteers ,
  IsForcingSupplies ,
  Siege ,
  Hideout ,
  SallyOut ,
  SiegeOutside
}
```

## Parties

```cs
MBReadOnlyList< MapEventParty >         PartiesOnSide (BattleSideEnum side)
IEnumerable< PartyBase >        InvolvedParties [get]
```

## Sides

```cs
MapEventSide    AttackerSide [get]
MapEventSide    DefenderSide [get]
```

## Player's event

```cs
static MapEvent         PlayerMapEvent [get]
```

<br><br>

----

## MapEventSide

* [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_map_events_1_1_map_event_side.html)

```cs
PartyBase       LeaderParty [get]
MBReadOnlyList< MapEventParty >         Parties [get]
BattleSideEnum  MissionSide [get]
int     TroopCount [get]
int     NumRemainingSimulationTroops [get]
float   CasualtyStrength [get]
MapEvent        MapEvent [get]
MapEventSide    OtherSide [get]
IFaction        MapFaction [get]
```

