# 🗲 Crashes 🗲


## NullReferenceException

    Message: Object reference not set to an instance of an object.

    Source: SandBox

??? failure "GetSuitableSpear - PrepareGuardAgentDataFromGarrison - TakeGuardAgentFromGarrisonTroopList - CreateStandGuardWithSpear"
    I'm getting this crash whenever I take a settlement with my custom culture and try to go to Lord's hall or dungeon
    <br>
    REASON: Custom culture guards entries in spgenericcharacters.xml (sandbox folder) need to go in spnpcharacters.xml in your mod's folder

</p>

    Source: TaleWorlds.CampaignSystem



??? failure "CampaignObjectManager - InitializeCachedData - InitializeOnNewGame - OnInitialize - DoLoadingForGameType"
    REASON: Settlement bound="Settlement.town_CR4" pointed to the non-existing settlement in settlements.xml
</p>


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

??? failure "DefaultMapDistanceModel - GetDistance - VillageGoodProductionCampaignBehavior - DistributeInitialItemsToTowns - OnNewGameCreatedPartialFollowUp"
    REASON: ??

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


??? failure "HeroCreator - CreateNewHero - FindRandomInternal - CreateNewHero - CreateSpecialHero"
    REASON: error in XSLT by assigning a culture to a hero

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

??? failure "ArrangeDestructedMeshes - SetUpScene - AfterStart - FinishMissionLoading"
    REASON: siege engine can't find which wall segment to attack. Tag is missing. Commented out in xscene:
    ``` xml
    <game_entity name="easteurope_wall_level_l1_right" old_prefab_name="">
        <flags>
            <flag name="record_to_scene_replay" value="true"/>
        </flags>
        <tags>
            <tag name="right_wall"/>  <--- this was used by siege_engine >
        </tags>
        ...
    ```

??? failure "DestructableComponent - OnInit"
    The scene was working on L1 Summer/Winter/Sprint, crashing on L1/Fall and on all L2/L3<br>
    REASON: entity with DestructableComponent had debris_holder with season_mask="253". Removing the season_mask solved the crash:
    ``` xml
    <!-- <game_entity name="debris_holder" old_prefab_name="" mobility="1" season_mask="253"> FALL CRASH -->
    <game_entity name="debris_holder" old_prefab_name="" mobility="1"> <!-- NO CRASH -->
    ```

??? failure "BackstoryCampaignBehavior - OnNewGameCreated"
    REASON: commented out some native hard-coded lord/settlement <br>
    Check in BackstoryCampaignBehavior.OnNewGameCreated <br>
    Fix with Harmony to skip this native code:

    ``` cs
    [HarmonyPatch(typeof(BackstoryCampaignBehavior))]
    [HarmonyPatch("OnNewGameCreated")]
    public class BackstoryCampaignBehavior_OnNewGameCreated_Patch
    {
        public static bool Prefix()
        {
            return false;
        }
    }
    ```


??? failure "CreateNewHero - CreateSpecialHero -  OnNewGameCreatedPartialFollowUp - InitialChildGenerationCampaignBehavior"
    REASON: Hero present, lord not present, lord used in a clan definition as owner

??? failure "DefaultMapDistanceModel - GetDistance - UpdateFriendshipAndEnemies"
    REASON: Lord/hero without a proper clan (clan not created/deleted)

??? failure "Kingdom - OnNewGameCreated - InvokeList - OnNewGameCreated"
    REASON: Deleted hero/lord, the 'owner' of the Kingdom


??? failure "CalculateDailyProductionAmount - GetWerehouseCapacity - TickProductions - OnNewGameCreatedPartialFollowUp"
    REASON: assigned a settlement to non-existant clan

??? failure "NameGenerator - CalculateNameScore - SelectNameIndex - GenerateHeroFirstName - CreateSpecialHero - InitialChildGenerationCampaignBehavior - OnNewGameCreatedPartialFollowUp"
    REASON: Clan's owner was non-existant Lord

??? failure "DefaultMapDistanceModel - GetClosestSettlementForNavigationMesh - GetDistance - FindNearestSettlement - TryToAssignTradeBoundForVillage - UpdateTradeBounds"
    REASON: saved a Main_map when game was open, xscene file gone, map gone. Game started with vanilla Main_map with custom settlements.xml

??? failure "AgingCampaignBehavior - OnHeroComesOfAge - InvokeList - DailyTickHero"
    REASON: error/missing sandboxcore_equipment_sets.xml for the hero's culture. Hero turned 18
</p>

    Source: MonoMod.Utils

??? failure "GauntletMovie - LoadMovie_Patch0 - Load - LoadMovie"
    REASON: Movie XML file not present. (deleted/wrong name/path?)


??? failure "LordConversationsCampaignBehavior - conversation_wanderer_introduction_on_condition - RunCondition"
    REASON: No native settlement town_ES4 in the game.


<br><br>
## ArgumentNullException

    Message: Value cannot be null. Parameter name: source
    Source: System.Core

??? failure "CreateHeroAtOccupation"
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



??? failure "SortedList.Add - AiVisitSettlementBehavior - FindSettlementsToVisitWithDistances - AiHourlyTick"
    An entry with the same key already exists
    <br>
    REASON: had &lt;Village> with the same id in the settlements.xml


<br><br>
## InvalidOperationException

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

??? failure "Single - CharacterCreationCultureStageVM - SortCultureList - CharacterCreationCultureStageView"
    REASON: Native culture is removed or is made is_main_culture=true


<br><br>
## IndexOutOfRangeException

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



<br><br>
## AccessViolationException

    Message: Attempted to read or write protected memory. This is often an indication that other memory is corrupt.
    Source: TaleWorlds.MountAndBlade


??? failure "MapScene - GetNavigationMeshCenterPosition - DefaultMapDistanceModel - GetClosestSettlementForNavigationMesh - GetDistance - CalculatePartyInfluenceCost - CalculateTotalInfluenceCost"
    REASON: CUSTOM_settlements.xml does not match settlements.xml<br>

</p>



??? failure "FinalizeMission - EndMissionInternal - CheckMissionEnd - OnTick"
    Crash was on the exit out from the mission. <br>
    REASON: xscene contained one entity, and commenting it out stopped the crash, no idea why/how, there are more similar entities in the scene:
    ``` xml
    <game_entity name="aserai_grandbazaar_wall" old_prefab_name="" occlusion_body_name="bo_occ_aserai_grandbazaar_wall">
        <transform position="-73.488, 137.875, 0.924" rotation_euler="0.000, 0.000, 1.571" scale="0.470, 0.150, 0.000"/>
        <physics shape="bo_aserai_grandbazaar_wall"/>
        <components>
            <meta_mesh_component name="aserai_grandbazaar_wall">
                <mesh name="aserai_grandbazaar_wall" material="adobe_wall_white"/>
            </meta_mesh_component>
        </components>
    </game_entity>
    ```


<br><br>
## Reflection.TargetInvocationException

    Message: Exception has been thrown by the target of an invocation.
    Source: mscorlib

??? failure "InvokeMethod - Invoke - CreateInstanceImpl - CreateInstance - CreateScreen"
    REASON: map xscene settlement ID mismatch with settlements.xml settlement ID. settlements.xml has a settlement with ID, which is not present in the xscene file

??? failure "... GetEncyclopediaPageInstance - SetEncyclopediaPage - GauntletMapEncyclopediaView - ExecuteLink"
    Crash by pressing on the lord's image in the Encyclopedia. <br>
    REASON: Lord's spause is deleted, but reference to it in the heroes.xml remains for the main lord. Eg: spouse="Hero.lord_4_2"










<br>
## TypeInitializationException

    Source: TaleWorlds.CampaignSystem

??? failure "CalculateBaseSpeed - CalculateSpeedForPartyUnified"
    More info [here](/modding/harmony/#patching-game-models)


<br><br>
## CTD without any message

On mission start.<br>
Error log last line: Selected formations being cleared.<br>
REASON: this entity was commented out in the xscene:
``` xml
<game_entity name="icon_camera" old_prefab_name="icon_camera" mobility="1">
```


--------


<br><br>
## Others




??? failure "GetBodyProperties - LocationCharacter - CreateMercenary - AddLocationCharacters - AddMercenaryCharacterToTavern"
    In CUSTOM_culture.xml error in defining caravan guards:
    ``` xml
    caravan_master="NPCCharacter.caravan_master_sturgia"
    armed_trader="NPCCharacter.armed_trader_sturgia"
    caravan_guard="NPCCharacter.caravan_guard_sturgia"
    veteran_caravan_guard="NPCCharacter.veteran_caravan_guard_sturgia"

    ```
