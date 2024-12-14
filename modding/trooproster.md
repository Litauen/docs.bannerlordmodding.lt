# Troop Roster

* [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_roster_1_1_troop_roster.html)

## Totals

* `Count` - count heroes + count of separate types of ordinary units. Example: 2 heroes + 11 Aserai Tribesman, .Count = 3
* `TotalRegulars`
* `TotalWoundedRegulars`
* `TotalWoundedHeroes`
* `TotalHeroes`
* `TotalWounded`
* `TotalManCount` - all units together with heroes, also wounded
* `TotalHealthyCount` - healty units

## Kill/Wound

``` cs
bool KillOneNonHeroTroopRandomly()
void KillNumberOfNonHeroTroopsRandomly(int numberOfMen)
void WoundNumberOfTroopsRandomly(int numberOfMen)
void WoundTroop (CharacterObject troop, int numberToWound=1, UniqueTroopDescriptor troopSeed=default(UniqueTroopDescriptor))
```