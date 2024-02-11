# XML Modding

![](/pics/yZvCYRb.png)

* spcultures.xml - [Cultures](/modding/cultures/)
* heroes.xml / lords.xml - [Heroes](/modding/heroes/#herolord-xml)
* settlements.xml - [Settlements](/modding/settlements/#xml)
* spclans.xml - [Clans](/modding/clans/)
* spkingdoms.xml - [Kingdoms](/modding/kingdoms/)
* npc/characters/templates - [NPC Character](/modding/npc_character)
* items - [Items](/modding/items/#xml)

## Notes

- If you change any native xml stuff you need to remove it with an xslt as well

## Crashes


### Type: System.ArgumentNullException

    Message: Value cannot be null. Parameter name: source
    Source: System.Core

??? failure " Value cannot be null. Parameter name: source"
    **CreateHeroAtOccupation**
    <br><br>
    REASON: in CUSTOM_culture.xml commented out one name:
    ``` xml
    <male_names>
        <!-- <name name="Adalbert" /> -->
        <name name="Adalbern" />
    ```
    also commented out one basic_mercenary_troops:
    ``` xml
    <basic_mercenary_troops>
        <template name="NPCCharacter.eastern_mercenary" />
        <template name="NPCCharacter.western_mercenary" />
        <!--<template name="NPCCharacter.sword_sisters_sister_t3" /> -->
    </basic_mercenary_troops>
    ```



??? failure "An entry with the same key already exists"
    **SortedList.Add - AiVisitSettlementBehavior - FindSettlementsToVisitWithDistances - AiHourlyTick**
    <br>
    REASON: had &lt;Village> with the same id in the settlements.xml

### Type: System.InvalidOperationException

    Message: The XmlReader state should be Interactive.
    Source: System.Xml.Linq

??? failure "ReadContentFrom - Load - ToXDocument - MergeTwoXmls - CreateMergedXmlFile"
    REASON: error in the XML file, example:<br>
    - missing opening tag &lt;Items><br>
    - wrong closing tag &lt;Items/> vs &lt;/Items><br>
    - CUSTOM_settlements.xml completely empty<br>
    - etc

</p>

    Message: Sequence contains no matching element
    Source: System.Core

??? failure "Enumerable.First - InitializeCaravanOnCreation - CreateCaravanParty - CreateParty - CreateCaravanParty"

    REASON: CUSTOM_culture notable Merchant was expecting CaravanGuard with the same culture: character.Culture == mobileParty.Party.Owner.Culture
    <br>
    ``` xml
    InitializeCaravanOnCreation
    CharacterObject characterObject = CharacterObject.All.First((CharacterObject character) => character.Occupation == Occupation.CaravanGuard && character.IsInfantry && character.Level == 26 && character.Culture == mobileParty.Party.Owner.Culture);
    ```


### Type: System.NullReferenceException

    Message: Object reference not set to an instance of an object.

??? failure "InitializeCachedData - InitializeOnNewGame - OnInitialize - DoLoadingForGameType"
    REASON: Settlement bound="Settlement.town_CR4" pointed to the non-existing settlement in settlements.xml
</p>

    Source: SandBox

??? failure "GetSuitableSpear - PrepareGuardAgentDataFromGarrison - TakeGuardAgentFromGarrisonTroopList - CreateStandGuardWithSpear"
    I'm getting this crash whenever I take a settlement with my custom culture and try to go to Lord's hall or dungeon
    <br>
    REASON: Custom culture guards entries in spgenericcharacters.xml (sandbox folder) need to go in spnpcharacters.xml in your mod's folder

</p>

    Source: TaleWorlds.CampaignSystem

??? failure "GetHeroesForEffectiveRelation - ApplyInternal - CreateRebelPartyAndClan - StartRebellionEvent - DailyTickSettlement"
    REASON: ?


??? failure "BuildWorkshopForHeroAtGameStart - BuildWorkshopsAtGameStart - OnNewGameCreatedPartialFollowUp"
    REASON: Used non-existant equipment set for NPCCharacter: &lt;EquipmentSet id="npc_gentry_equipment_sturgia"/>

??? failure "GetBodyProperties - GetCharacterCode - set_Character - set_Troop - PartyCharacterVM - InitializePartyList"
    REASON: Used non-existant BodyProperty for NPCCharacter


??? failure "DefaultMapDistanceModel - GetDistance"
    REASON1: Wrongly assigned Settlement, Kingdom without a settlement. Attach dnSpy, it will show faction.FactionMidSettlement == null. Fix your XML.
    <br><br>
    REASON2: Problems with Navmesh. Inaccessible settlements. Old/mismatching settlements_distance_cache.bin - fix navmesh, regenerate settlements_distance_cache.bin
    <br><br>
    **CampaignObjectManager.InitializeCachedData()**
    <br><br>
    REASON: settlements.xml error - I accidently deleted one settlement and village had no bounded castle (crash on settlement.OwnerClan.OnBoundVillageAdded(settlement.Village);). This was on new game start.

??? failure "SpawnNotablesIfNeeded - NotablesCampaignBehavior - DailyTickSettlement"
    REASON: Some type of notable is missing for some culture. I was missing RuralNotable<br>
    Set in &lt;notable_and_wanderer_templates> in culture XML

??? failure "CalculateAverageWage - DoLoadingForGameType - DoLoadingForGameManager"
    REASON: ??? (maybe something related to party templates with non-existant troop IDs, but not confirmed)

??? failure "CharacterObject - GetSkillValue - DefaultPartyMoraleModel - GetMoraleEffectsFromSkill - GetEffectivePartyMorale"
    REASON: used not existant troop ID in the &lt;MBPartyTemplate id="villager_CUSTOM_CULTURE_template"><br>
    HINT: Check Encyclopedia-Troops if necessary troop is actually in the game.

??? failure "DefaultPartyWageModel - GetTotalWage - MobileParty - get_TotalWage - get_LimitedPartySize - CalculateGarrisonChangeInternal"
    REASON: ???

??? failure "SetInitialValuesFromCharacter - CreateNewHero - CreateSpecialHero - CreateMinorFactionHeroFromTemplate - SpawnMinorFactionHeroes - OnNewGameCreated"
    REASON: spclans.xslt and custom_spclans.xml were in a separate folder for spclans, but missing empty spclans.xml file


??? failure "SiegeTowerSpawner - AssignParameters - SpawnerEntityMissionHelper - OnPreInit"
    On starting the scene. <br>
    REASON: name property for siege_tower entity:
    ``` xml
    <game_entity name="siege_tower_5m_spawner_right" prefab="siege_tower_5m_spawner">
    ```
    removed name=... solved the crash:
    ``` xml
    <game_entity prefab="siege_tower_5m_spawner">
    ```


### Type: System.IndexOutOfRangeException

    Message: Index was outside the bounds of the array.
    Source: TaleWorlds.CampaignSystem 

??? failure "DefaultMapWeatherModel - GetWeatherEventInPosition - WeatherAudioTick - TickVisuals"

    REASON: Marked 'Use Dynamic Weather Effects' without the a flowmap. Maybe map is not a square. Maybe flowmap is not 1024x1024.

    ![](/pics/202402032219.png)

---

    Source: TaleWorlds.CampaignSystem

??? failure "OnNewGameCreated"
    REASON1: Lord (NPCCharacter) id mismatch with hero id, no lord for the heroe, "Hero." missed with hero id
    <br><br>
    REASON2: 2 lords with the same id
    <br><br>
    REASON3: Comments in spkingdoms.xml :D
    ![](/pics/teP7QtL.png)
    <br><br>
    REASON4: NPCCharacter age="9.9" (not int) in lords.xml

??? failure "OnHeroComesOfAge"
    REASON: error/missing sandboxcore_equipment_sets.xml for the hero's culture. Hero turned 18

---

    Others

??? failure "UpdateFriendshipAndEnemies"
    Lord/hero without the proper clan (clan not created)

??? failure "InitialChildGenerationCampaignBehavior - OnNewGameCreatedPartialFollowUp - CreateSpecialHero - CreateNewHero"
    Hero present, lord not present, lord used in a clan definition (owner)


??? failure "GetBodyProperties - LocationCharacter - CreateMercenary - AddLocationCharacters - AddMercenaryCharacterToTavern"
    In CUSTOM_culture.xml error in defining caravan guards:
    ``` xml
    caravan_master="NPCCharacter.caravan_master_sturgia"
    armed_trader="NPCCharacter.armed_trader_sturgia"
    caravan_guard="NPCCharacter.caravan_guard_sturgia"
    veteran_caravan_guard="NPCCharacter.veteran_caravan_guard_sturgia"

    ```

    System.AccessViolationException

??? failure "MapScene - GetNavigationMeshCenterPosition - DefaultMapDistanceModel - GetClosestSettlementForNavigationMesh"
    REASON: CUSTOM_settlements.xml does not match settlements.xml


### Type: System.Reflection.TargetInvocationException

    Message: Exception has been thrown by the target of an invocation.
    Source: mscorlib

??? failure "InvokeMethod - Invoke - CreateInstanceImpl - CreateInstance - CreateScreen"
    REASON: map xscene settlement ID mismatch with settlements.xml settlement ID. settlements.xml has a settlement with ID, which is not present in the xscene file




## Formatting

??? success "Make ugly XMLs easier to work with!"
    ![](/pics/xENrxdr.png)

??? hint "Notepad++ with XML Tools plugin"
    ![](/pics/M6QGjrP.png)

Online tools:

- [Make it readable](https://www.liquid-technologies.com/online-xml-formatter){target=_blank}
- [Fix Tab spaces](https://jsonformatter.org/xml-formatter){target=_blank}







