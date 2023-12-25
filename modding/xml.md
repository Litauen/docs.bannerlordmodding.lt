# XML Modding

![](https://imgur.com/yZvCYRb.png)


## Crashes

??? failure "System.InvalidOperationException Sequence contains no matching element"
    **TSource System.Linq.Enumerable.First(IEnumerable source, Func predicate) / CaravanPartyComponent.InitializeCaravanOnCreation**
    <br><br>
    REASON: CUSTOM_culture notable Merchant was expecting CaravanGuard with the same culture: character.Culture == mobileParty.Party.Owner.Culture
    <br><br>
    ``` xml
    InitializeCaravanOnCreation
    CharacterObject characterObject = CharacterObject.All.First((CharacterObject character) => character.Occupation == Occupation.CaravanGuard && character.IsInfantry && character.Level == 26 && character.Culture == mobileParty.Party.Owner.Culture);
    ```

??? failure "System.ArgumentNullException Value cannot be null. Parameter name: source"
    **CreateHeroAtOccupation**
    <br><br>
    REASON: in CUSTOM_culture.xml commented out one name:
    ``` xml
    <male_names>
        <!-- <name name="Adalbert" /> -->
        <name name="Adalbern" />
    ```

??? failure "Object reference not set to an instance of an object"
    **GetBodyProperties - LocationCharacter - CreateMercenary - AddLocationCharacters - AddMercenaryCharacterToTavern**
    <br><br>
    REASON: In CUSTOM_culture.xml error in defining caravan guards:
    ``` xml
    caravan_master="NPCCharacter.caravan_master_sturgia"
    armed_trader="NPCCharacter.armed_trader_sturgia"
    caravan_guard="NPCCharacter.caravan_guard_sturgia"
    veteran_caravan_guard="NPCCharacter.veteran_caravan_guard_sturgia"

    ```

## Culture

These lines in CUSTOM_culture.xml define NPCs hirable in the Tavern, not the actual Caravan Guards that travel the world map.

``` xml
caravan_master="NPCCharacter.caravan_master_crusader"
armed_trader="NPCCharacter.armed_trader_crusader"
caravan_guard="NPCCharacter.caravan_guard_crusader"
veteran_caravan_guard="NPCCharacter.veteran_caravan_guard_crusader"
```

Traveling Caravan Guard NPCs on the world map are generated from the same culture troop as Town's notable it belongs to, and must satisfy the following conditions:

``` xml
Occupation.CaravanGuard && character.IsInfantry && character.Level == 26 && character.Culture == mobileParty.Party.Owner.Culture
```