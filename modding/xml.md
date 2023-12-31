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

### Object reference not set to an instance of an object

??? failure "GetBodyProperties - LocationCharacter - CreateMercenary - AddLocationCharacters - AddMercenaryCharacterToTavern"
    In CUSTOM_culture.xml error in defining caravan guards:
    ``` xml
    caravan_master="NPCCharacter.caravan_master_sturgia"
    armed_trader="NPCCharacter.armed_trader_sturgia"
    caravan_guard="NPCCharacter.caravan_guard_sturgia"
    veteran_caravan_guard="NPCCharacter.veteran_caravan_guard_sturgia"

    ```

??? failure "OnNewGameCreated"
    REASON1: Lord (NPCCharacter) id mismatch with hero id, no lord for the heroe, "Hero." missed with hero id
    <br><br>
    REASON2: 2 lords with the same id



### NullReferenceException

??? failure "UpdateFriendshipAndEnemies"
    Lord/hero without the proper clan (clan not created)

??? failure "InitialChildGenerationCampaignBehavior.OnNewGameCreatedPartialFollowUp - CreateSpecialHero - CreateNewHero"
    Hero present, lord not present, lord used in clan definition (owner)

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

Culture can exist without the female clan member (it was stated somewhere that otherwise).

## Hero/Lord

Add entry into heroes.xml

``` xml
<Hero id="lord_lit_1_1" spouse="Hero.lord_lit_1_2" father="Hero.lit_1_0" mother="Hero.lit_1_00" alive="false" faction="Faction.clan_lit_1" />
```

Add entry into lords.xml

``` xml

    <NPCCharacter ...
```

??? question "How to properly make lord dead?"
    alive="false" makes him dead, but in description it writes: HERO_NAME is  member of ...


## Kingdom

Kingdom can exists without owning a Town. Castle is enough. (It was stated somewhere otherwise)


