# 🗲 Crashes 🗲


## NullReferenceException

    Message: Object reference not set to an instance of an object.


??? failure "BesiegerCamp - RemoveSiegePartyInternal - OnPartyLeftSiegeInternal ... RemoveParty"
    REASON: ???

??? failure "GetBornSettlement - DefaultHeroCreationModel - InitializeHeroFromSettings - DeliverOffSpring"
    REASON: ???

??? failure "GetCivilianEquipments - DefaultHeroCreationModel - InitializeHeroFromSettings - DeliverOffSpring"
    REASON: Missing equipment template from `sandbox_equipment_sets`, like `child_template_baltic_noble_female`

??? failure "CharacterObject - GetSkillValue - AddSkillBonusForParty - DefaultPartyTradeModel - GetTradePenaltyFactor"
    REASON: No caravan troops defined.

??? failure "MergeElements - CreateMergedXmlFile - LoadXML"
    REASON: Error in the XML file. Check the RGL log to see in which one.

??? failure "HeroCreator - CreateHero - CreateNotable - SpawnNotablesAtGameStart"
    REASON1: No notable template<br>
    REASON2: Used Hero.id that is not defined yet, e.g. mother="Hero.dead_lord_lit_5_2"<br>
    REASON3: No EquipmentSet for a lord<br>
    REASON4: Used non-existant culture for a lord

??? failure "GetSuitableSpear - PrepareGuardAgentDataFromGarrison - TakeGuardAgentFromGarrisonTroopList - CreateStandGuardWithSpear"
    I'm getting this crash whenever I take a settlement with my custom culture and try to go to Lord's hall or dungeon
    <br>
    REASON: Custom culture guards entries in spgenericcharacters.xml (sandbox folder) need to go in spnpcharacters.xml in your mod's folder


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
    Once I loaded Lemmy's map together with mine and got this crash ¯\\\_(ツ)\_/¯
    <br><br>
    **CampaignObjectManager.InitializeCachedData()**
    <br><br>
    REASON: settlements.xml error - I accidently deleted one settlement and village had no bounded castle (crash on settlement.OwnerClan.OnBoundVillageAdded(settlement.Village);). This was on new game start.
    <br>
    ??? example "If nothing helps, use this patch:"
        ``` cs
        [HarmonyPatch(typeof(DefaultMapDistanceModel))]
        [HarmonyPatch("GetDistance")]
        [HarmonyPatch(new Type[] { typeof(Settlement), typeof(Settlement) })]
        public class DefaultMapDistanceModel_GetDistance_Patch
        {
            static bool Prefix(Settlement fromSettlement, Settlement toSettlement, ref float __result)
            {
                if (fromSettlement == null || toSettlement == null)
                {
                    __result = 0f;
                    return false;
                }
                return true;
            }
        }
        ```


??? failure "DefaultMapDistanceModel - GetDistance - VillageGoodProductionCampaignBehavior - DistributeInitialItemsToTowns - OnNewGameCreatedPartialFollowUp"
    REASON: it happens when a village is too far away from a city (even if the village belongs to a castle) but there is a script to fix that <br>
    SOLUTION: : The problem solved by having to make a town closer to the castle, and raise the trade bound system to 4000 ([more info](https://discord.com/channels/411286129317249035/761302555308720148/1235320061468868709))

??? failure "SpawnNotablesIfNeeded - NotablesCampaignBehavior - DailyTickSettlement"
    REASON: Some type of notable is missing for some culture. I was missing RuralNotable<br>
    Set in &lt;notable_and_wanderer_templates> in culture XML

??? failure "CalculateAverageWage - DoLoadingForGameType - DoLoadingForGameManager"
    REASON: ??? (maybe something related to party templates with non-existant troop IDs, but not confirmed)

??? failure "CharacterObject - GetSkillValue - DefaultPartyMoraleModel - GetMoraleEffectsFromSkill - GetEffectivePartyMorale"
    REASON1: used not existant troop ID in the &lt;MBPartyTemplate id="villager_CUSTOM_CULTURE_template"><br>
    HINT: Check Encyclopedia-Troops if necessary troop is actually in the game.<br>
    <br>
    REASON2: When you get this error after character creation, it's because trooptrees/their equipments have either a double "/>/>" somewhere, or because there is invalid slot ID (e.g. "Shoulder" instead of correct "Cape") (Bubu)

??? failure "DefaultPartyWageModel - GetTotalWage - MobileParty - get_TotalWage - get_LimitedPartySize - CalculateGarrisonChangeInternal"
    REASON: Troop ID used in partyTemplates.xml refers to a non-existing troop ID in the trooptree. 

??? failure "SetInitialValuesFromCharacter - CreateNewHero - CreateSpecialHero - CreateMinorFactionHeroFromTemplate - SpawnMinorFactionHeroes - OnNewGameCreated"
    REASON1: spclans.xslt and custom_spclans.xml were in a separate folder for spclans, but missing empty spclans.xml file<br>
    REASON2: for Lord NPCCharacter used EquipmentSet instead of EquipmentRoster


??? failure "HeroCreator - CreateNewHero - CreateSpecialHero - CreateMinorFactionHeroFromTemplate - SpawnMinorFactionHeroes - OnNewGameCreated"
    REASON: error in &lt;minor_faction_character_templates>, I used name= instead of id=



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


??? failure "InitialChildGenerationCampaignBehavior - OnNewGameCreatedPartialFollowUp - InvokeList - OnNewGameCreated"
    REASON: Missing male or female lord of one of the cultures. Both male and female lords should be present for each culture to pass this function at the game creation.

??? failure "CreateNewHero - CreateSpecialHero -  OnNewGameCreatedPartialFollowUp - InitialChildGenerationCampaignBehavior"
    REASON: Hero present, lord not present, lord used in a clan definition as owner<br>
    REASON2: wrong id in &lt;xsl:template match="Settlement[@id='village_EN1_3']/@culture">


??? failure "DefaultMapDistanceModel - GetDistance - UpdateFriendshipAndEnemies"
    REASON 1: Lord/hero without a proper clan/faction (faction not created/deleted/error in the ID). RGL log can show the error.
    <br><br>
    REASON 2: Incorrect load order when launching game, e.g. Lemmy's map was loaded after a mod using it.

??? failure "Kingdom - OnNewGameCreated - InvokeList - OnNewGameCreated"
    REASON: Deleted hero/lord, the `owner` of the Kingdom<br>
    REASON2: in `heroes.xml` for hero wrote `spouse="lord_prus_4_3"`, `should be spouse="Hero.lord_prus_4_3"`<br>
    REASON3: `<relationships>` was empty in `kingdoms.xml` for a new kingdom


??? failure "CalculateDailyProductionAmount - GetWerehouseCapacity - TickProductions - OnNewGameCreatedPartialFollowUp"
    REASON: assigned a settlement to non-existant clan<br>
    REASON2: error in defining minor_clan, I had label_color="FF7264D16" (one digit too many in the color code)<br>
    REASON3: error in defining a clan, had NaN in banner_key="11.141.19.1836.1836.768.788.1.0.NaN.510...., wrong/duplicate Faction id=<br>



??? failure "MapScreen.OnInitialize - ScreenBase.HandleInitialize - CleanAndPushScreen"
    REASON: deleted such __empty_entity from the World Map (by BuBu):
    ```xml
    <game_entity name="__empty_object" old_prefab_name="__empty_object" mobility="1">
        <transform position="699.963, 750.106, 2.149" rotation_euler="0.000, 0.000, 0.000"/>
        <scripts>
            <script name="CampaignMapSiegePrefabEntityCache">
                <variables>
                    <variable name="_attackerBallistaPrefab" value="ballista_a_mapicon"/>
                    <variable name="_defenderBallistaPrefab" value="ballista_b_mapicon"/>
                    <variable name="_attackerFireBallistaPrefab" value="ballista_a_fire_mapicon"/>
                    <variable name="_defenderFireBallistaPrefab" value="ballista_b_fire_mapicon"/>
                    <variable name="_attackerMangonelPrefab" value="mangonel_a_mapicon"/>
                    <variable name="_defenderMangonelPrefab" value="mangonel_b_mapicon"/>
                    <variable name="_attackerFireMangonelPrefab" value="mangonel_a_fire_mapicon"/>
                    <variable name="_defenderFireMangonelPrefab" value="mangonel_b_fire_mapicon"/>
                    <variable name="_attackerTrebuchetPrefab" value="trebuchet_a_mapicon"/>
                    <variable name="_defenderTrebuchetPrefab" value="trebuchet_b_mapicon"/>
                </variables>
            </script>
        </scripts>
    </game_entity>
    ```

??? failure "NameGenerator - CalculateNameScore - SelectNameIndex - GenerateHeroFirstName - CreateSpecialHero - InitialChildGenerationCampaignBehavior - OnNewGameCreatedPartialFollowUp"
    REASON: Clan's owner was non-existant Lord<br>
    REASON2: Created heroes (Hero), forgot to create npc_lords (NPCCharacter)

??? failure "DefaultMapDistanceModel - GetClosestSettlementForNavigationMesh - GetDistance - FindNearestSettlement - TryToAssignTradeBoundForVillage - UpdateTradeBounds"
    REASON: saved a Main_map when game was open, xscene file gone, map gone. Game started with vanilla Main_map with custom settlements.xml

??? failure "AgingCampaignBehavior - OnHeroComesOfAge - InvokeList - DailyTickHero"
    REASON: error/missing sandboxcore_equipment_sets.xml for the hero's culture. Hero turned 18

??? failure "TournamentGames - TournamentMatch - AddParticipant - TournamentBehavior - FillParticipants"
    REASON: RBM Tournament expects local culture troops in the settlement for the Tournament. Not all tiers (0-6) of troops are implemented for that culture. Check your troop tree.

??? failure "Clan - OnFortificationAdded - PreAfterLoad"
    REASON: Old save loaded on a new map with new settlements not present in the old save

??? failure "GangLeaderNeedsToOffloadStolenGoodsIssueBehavior - ConditionsHold - OnCheckForIssue - IssuesCampaignBehavior - CreateAnIssueForSettlementNotables"
    REASON: It's seems it is related to the mission NotableWantsDaughterFound. When it ends the GangLeader is spawned in the village where this mission was originated. This GangLeader then is used for this OffloadStolenGoods mission check and that results in the crash. Seems like native bug. I patched the crashing method with the check if village notable is not GangLeader.

??? failure "ApplySimulatedHitRewardToSelectedTroop - SimulateSingleHit - SimulateBattleForRound"
    Crash in CharacterObject.get_FirstBattleEquipment<br>
    REASON: troop with all &lt;EquipmentRoster civilian="true">, without &lt;EquipmentRoster>

??? failure "CanTroopGainXP - MobilePartyHelper - OnBattleEnd - DefaultSkillLevelingManager"
    REASON: character(troop?) with no upgrade targets?<br>
    [Mod with fix](https://www.nexusmods.com/mountandblade2bannerlord/mods/7232)

??? failure "GetEncounterTargetPoint - HandleEncounterForMobileParty - HandleEncounters"
    REASON: [Complex Characters mod](https://www.nexusmods.com/mountandblade2bannerlord/mods/7565) - try without it. <br>
    NOTE: Impossible to troubleshoot, because mod is obfuscated.

??? failure "GetIssueEffectsOfSettlement - CalculateTownFoodChangeInternal"
    Crash on save file load.<br>
    REASON: Settlements on the map do not match settlements in the save file. (Old/incompatible save?)


</p>


    Source: MonoMod.Utils

??? failure "GauntletMovie - LoadMovie_Patch0 - Load - LoadMovie"
    REASON: Movie XML file not present. (deleted/wrong name/path?)


??? failure "LordConversationsCampaignBehavior - conversation_wanderer_introduction_on_condition - RunCondition"
    REASON: No native settlement town_ES4 in the game.


??? failure "GetEffectiveDailyExperience - DefaultPartyTrainingModel - OnDailyTickParty"
    REASON: melee_elite_militia_troop in culture.xml was set to non-existant troop ID



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
    <b>Message: An entry with the same key already exists</b>
    <br>
    REASON: had &lt;Village> with the same id in the settlements.xml (not Settlement id but Village id with the \_comp_!)

    ??? example "Simple bash script to check for 'Village id' duplicates:"
        ``` bash
        #!/bin/bash

        # Define the XML file path
        XML_FILE="settlements.xml"

        # Check if the file exists
        if [[ ! -f "$XML_FILE" ]]; then
            echo "File $XML_FILE does not exist."
            exit 1
        fi

        # Extract id attributes from Village elements
        ids=$(xmllint --xpath '//Village/@id' "$XML_FILE" 2>/dev/null | grep -o 'id="[^"]*"' | sed 's/id=//g' | tr -d '"')

        # Check if xmllint failed
        if [[ $? -ne 0 ]]; then
            echo "Error processing the XML file."
            exit 1
        fi

        # Find duplicate ids
        duplicate_ids=$(echo "$ids" | sort | uniq -d)

        # Display the duplicate ids
        if [[ -n "$duplicate_ids" ]]; then
            echo "Duplicate Village ids found:"
            for id in $duplicate_ids; do
                count=$(echo "$ids" | grep -c "^$id$")
                echo "id: $id, Count: $count"
            done
        else
            echo "No duplicate Village ids found."
        fi
        ```


??? failure "Linq.Enumerable.Any - Extensions.IsEmpty - NameGenerator - GetNameListForCulture - GenerateHeroFirstName"
    REASON: error in the culture xml file







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
    SOLUTION: create CaravanGuard in your new culture<br>
    and match this: Occupation.CaravanGuard && character.IsInfantry && character.Level == 26

??? failure "Single - CharacterCreationCultureStageVM - SortCultureList - CharacterCreationCultureStageView"
    REASON: Native culture is removed or is made is_main_culture=true


<br><br>
## IndexOutOfRangeException

    Message: Index was outside the bounds of the array.
    Source: TaleWorlds.CampaignSystem 

??? failure "DefaultMapWeatherModel - GetWeatherEventInPosition - WeatherAudioTick - TickVisuals"

    Usually crash when going to the very edge of the map.

    REASON: Marked 'Use Dynamic Weather Effects' without the a flowmap.<br>
    OTHER REASONS: Maybe map is not a square. Maybe flowmap is not 1024x1024.

    ![](/pics/202402032219.png)

    ??? abstract "If nothing helps, use this patch:"
        ``` cs
        [HarmonyPatch(typeof(DefaultMapWeatherModel), "GetWeatherEventInPosition")]
        public class DefaultMapWeatherModel_GetWeatherEventInPosition_Patch
        {
            // Create a field reference for accessing the private _weatherDataCache field in DefaultMapWeatherModel
            private static readonly AccessTools.FieldRef<DefaultMapWeatherModel, MapWeatherModel.WeatherEvent[]> _weatherDataCacheRef =
                AccessTools.FieldRefAccess<DefaultMapWeatherModel, MapWeatherModel.WeatherEvent[]>("_weatherDataCache");

            static bool Prefix(Vec2 pos, ref MapWeatherModel.WeatherEvent __result, DefaultMapWeatherModel __instance)
            {
                // Define the variables that will be passed as out parameters
                int num, num2;

                // Use AccessTools to get the private method GetNodePositionForWeather
                MethodInfo getNodePositionMethod = AccessTools.Method(typeof(DefaultMapWeatherModel), "GetNodePositionForWeather");

                // Prepare the arguments for the method (since it's expecting out params, you should use a reference array)
                object[] methodArgs = new object[] { pos, null, null };

                // Invoke the method using reflection
                getNodePositionMethod.Invoke(__instance, methodArgs);

                // Extract the results from the out parameters
                num = (int)methodArgs[1];
                num2 = (int)methodArgs[2];

                // Calculate the index
                int index = num2 * 32 + num;

                // Access the private _weatherDataCache field using the field reference
                var weatherDataCache = _weatherDataCacheRef(__instance);

                // Check if the index is out of bounds
                if (index >= weatherDataCache.Length || index < 0) // Use array length for safety
                {
                    // Return WeatherEvent.Clear and skip the original method
                    __result = MapWeatherModel.WeatherEvent.Clear;
                    return false;
                }

                // Otherwise, return the cached weather data and skip the original method
                __result = weatherDataCache[index];
                return false;
            }
        }
        ```


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

---

??? failure "TickSiegeMachineCircles - MapScreen - OnFrameTick"
    Crash when starting to siege a settlement<br>
    REASON: Wrong number of siege engines on the map<br>
    ??? abstract "Troubleshooting example:"
        I used this Harmony patch to locate where is the problem:
        ``` cs
        [HarmonyPatch(typeof(MapScreen), "TickSiegeMachineCircles")]
        public class MapScreen_TickSiegeMachineCircles_Patch
        {
            static bool Prefix()
            {
                if (PlayerSiege.PlayerSiegeEvent == null) return false;

                SiegeEvent playerSiegeEvent = PlayerSiege.PlayerSiegeEvent;
                Settlement besiegedSettlement = playerSiegeEvent.BesiegedSettlement;
                PartyVisual visualOfParty = PartyVisualManager.Current.GetVisualOfParty(besiegedSettlement.Party);

                LTLogger.Debug($"visualOfParty.GetDefenderRangedSiegeEngineFrames().Length {visualOfParty.GetDefenderRangedSiegeEngineFrames().Length}");
                LTLogger.Debug($"playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Defender).SiegeEngines.DeployedRangedSiegeEngines.Length {playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Defender).SiegeEngines.DeployedRangedSiegeEngines.Length}");

                LTLogger.Debug($"visualOfParty.GetAttackerRangedSiegeEngineFrames().Length {visualOfParty.GetAttackerRangedSiegeEngineFrames().Length}");
                LTLogger.Debug($"playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Attacker).SiegeEngines.DeployedRangedSiegeEngines.Length {playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Attacker).SiegeEngines.DeployedRangedSiegeEngines.Length}");

                LTLogger.Debug($"visualOfParty.GetAttackerBatteringRamSiegeEngineFrames().Length {visualOfParty.GetAttackerBatteringRamSiegeEngineFrames().Length}");
                LTLogger.Debug($"playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Attacker).SiegeEngines.DeployedMeleeSiegeEngines.Length {playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Attacker).SiegeEngines.DeployedMeleeSiegeEngines.Length}");

                LTLogger.Debug($"visualOfParty.GetAttackerTowerSiegeEngineFrames().Length {visualOfParty.GetAttackerTowerSiegeEngineFrames().Length}");
                LTLogger.Debug($"playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Attacker).SiegeEngines.DeployedMeleeSiegeEngines.Length {playerSiegeEvent.GetSiegeEventSide(BattleSideEnum.Attacker).SiegeEngines.DeployedMeleeSiegeEngines.Length}");

                return true;
            }
        }
        ```
        LTLogger.Debug is my custom function to output values to the debug file.<br><br>
        Here it shows that there are too many attacker's Ranged Engines:
        ![](/pics/2409171025.png)
        <br>
        Editor shows the problem:<br>
        ![](/pics/2409171123.png)<br>
        Deleting one of them solves the crash.


<br><br>
## ArgumentOutOfRangeException

    Message: Index was out of range. Must be non-negative and less than the size of the collection. Parameter name: index

??? failure "TournamentFightMissionController - PrepareForMatch - SkipMatch"
    REASON: in culture's file used bad template for &lt;tournament_team_templates_one_participant>



<br><br>
## AccessViolationException

    Message: Attempted to read or write protected memory. This is often an indication that other memory is corrupt.
    Source: TaleWorlds.MountAndBlade


??? failure "MapScene - GetNavigationMeshCenterPosition - DefaultMapDistanceModel - GetClosestSettlementForNavigationMesh - GetDistance - CalculatePartyInfluenceCost - CalculateTotalInfluenceCost"
    REASON: settlement not covered by Navmesh<br>
    REASON2: [hole](https://discord.com/channels/411286129317249035/677511186295685150/1272497615140945991) in the Navmesh<br>
    REASON3: outdated Settlements Distance Cache. [Delete/regenerate it](/modding/settlements/#settlements-distance-cache)<br>
    <br>
    [hunharibo](https://discord.com/channels/411286129317249035/677511186295685150/1274288069528256608): looks like a navmesh issue. Either a face with inverted normals or a concave quad or a face that is not connected to anything

    REASON4: bad party with coordinates 0,0. I had this problem when Warlord or Deserters mod created deserters_... parties with coordinates 0,0. Game tries to get GetNavigationMeshCenterPosition(PathFaceRecord face) for those parties and crash. Example: https://report.butr.link/824A29

    !!! tip "Use [Settlement Position Script](/editor/settlementpositionscript/) to find problems with navmesh and settlement placement."

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

??? failure "GetMeshAtIndex - ScriptingInterfaceOfIMetaMesh - GetTableauMaterial - ItemCollectionElementViewExtensions"
    Crash in the inventory screen. <br>
    REASON: used the same mesh for a different shield when defining an &lt;Item>

??? failure "ScriptingInterfaceOfIMBMission - AddMissile - Mission.AddMissileAux"
    REASON: ??


??? failure "ScriptingInterfaceOfIMBMission.GetBoundaryPoints - MBBoundaryCollection.ContainsKey - SetCameraFrameToMapView"
    REASON: Trying to open InventoryManager.OpenScreenAsReceiveItems in a mission :) ([example](https://report.butr.link/D2E5C2))

<br><br>
## Reflection.TargetInvocationException

    Message: Exception has been thrown by the target of an invocation.
    Source: mscorlib

??? failure "InvokeMethod - Invoke - CreateInstanceImpl - CreateInstance - CreateScreen"
    REASON: map xscene settlement ID mismatch with settlements.xml settlement ID. settlements.xml has a settlement with ID, which is not present in the xscene file

??? failure "... GetEncyclopediaPageInstance - SetEncyclopediaPage - GauntletMapEncyclopediaView - ExecuteLink"
    Crash by pressing on the lord's image in the Encyclopedia. <br>
    REASON: Lord's spause is deleted, but reference to it in the heroes.xml remains for the main lord. Eg: spouse="Hero.lord_4_2"

??? failure "InvalidOperationException: Sequence contains no elements"
    Crash on accepting [LesserNobleRevolt](https://mountandblade.fandom.com/wiki/Lieutenant's_Revolt) mission. CTD on normal run.<br>
    REASON: bad troop ID in cultures.xml elite_basic_troop
    ??? failure "Crash report on VS:"
        ![](/pics/2409201550.png)




<br>
## TypeInitializationException

    Source: TaleWorlds.CampaignSystem

??? failure "CalculateBaseSpeed - CalculateSpeedForPartyUnified"
    More info [here](/modding/harmony/#patching-game-models)


<br>
## System.Xml.Xsl.XslTransformException

    Message: Attribute and namespace nodes cannot be added to the parent element after a text, comment, pi, or sub-element node has already been added.
    Source: System.Data.SqlXml

??? failure "XmlQueryOutput - ThrowInvalidStateError"
    REASON: tried to change non-existant attribute with XSLT


</p>

    Message: Unexpected token '=' in the expression.

??? failure "XslCompiledTransform - LoadInternal - ApplyXslt - CreateMergedXmlFile - GetMergedXmlForManaged - LoadXML - InitializeSandboxXMLs"
    REASON: Translation line in the xslt, like {=skalvians}Skalvians. Move this to XML


<br><br>
## KeyNotFoundException

    Message: The given key was not present in the dictionary.

??? failure "Dictionary - GetInfestedHideoutCount - BanditSpawnCampaignBehavior"
    REASON: some defined bandit faction has no hideout on the map




<br><br>
## CTD without any message

CTD - Crash To Desktop


----

    On new game start

REASON: non-existant troop refferenced in party_templates.xml &lt;PartyTemplateStack ... />

----

    On mission start
Error log last line: Selected formations being cleared.<br>
REASON: this entity was commented out in the xscene:
``` xml
<game_entity name="icon_camera" old_prefab_name="icon_camera" mobility="1">
```

----

    On entering scene
REASON: scene does not exist<br>
REASON2: error in the scene's xscene file

----
    When entering settlement
REASON: Bad troop id: villager_danish vs danish_villager


----
    When cheat mode ON and Party icon is pressed
REASON: upgrade_target for some troop set to non-existant troop

!!! abstract "In such a case log shows:"
    Exception occured inside invoke: ExecuteOpenParty<br>
    Target type: TaleWorlds.CampaignSystem.VieModelCollection.Map.MapBar.MapNavigationVM<br>
    Argument cound: 0<br>
    Inner message: Object reference not set to an instance of an object.


----
    CTD on the world map ~1 day after the start
REASON: Mercenary clan initial start location outside the playable area

Attaching dnSpy it shows that the crash happens in the `ScriptingInterfaceOfIScene-GetNavMeshFaceCenterPosition`

--------


<br><br>
## Hang/Freeze/Deadlock


??? failure "On Mission start"
    Made a mistake in shield description (body_name and shield_body_name missing 'bo' parts):
    ```xml
        <Item
        id="lt_teutonic_shield_anno"
        name="{=lt_teutonic_shield_anno}Anno von Sangershausen's Shield"
        body_name="lt_teutonic_shield_anno"
        shield_body_name="lt_teutonic_shield_anno"
    ```



--------
<br><br>
## d3d_device_context


![](/pics/2411120946.png)

Crashes most frequently occur during battles, specifically when opening the command menu during the troop deployment phase.

Potential solutions to try:

* Reset NVIDIA settings: NVIDIA Control Panel -> 3D Settings -> Manage 3D Settings -> Global Settings -> Restore Defaults
* Delete the Shaders folder: C:\ProgramData\Mount and Blade II Bannerlord\Shaders
* Lower shader quality (in the game): Options -> Performance -> Graphics -> Set Shader Quality to Medium
* If you're using an overclocked GPU, try reverting to default settings (some users reported crashes maybe due to overheating or insufficient voltage?)

[Source](https://steamcommunity.com/app/261550/discussions/13/3192490990765522541)

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

??? failure "HarmonyLib.PatchClassProcessor.ReportException() - Patch()"
    When loading the game with DnSpy attached, make sure to disable the BannerColorPersistence in the load order.



