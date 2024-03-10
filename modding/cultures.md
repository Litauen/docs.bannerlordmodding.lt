# Cultures

- [XML Culture doc](https://docs.bannerlordmodding.com/_xmldocs/spcultures/culture/)
- [Custom Culture Selection Screen](/guides/custom_culture_selection_screen/)

## Default cultures

* aserai
* battania
* empire
* knuzait
* sturgia
* vlandia



## Check

    if (hero.Culture.StringId == "sturgia") ..


## Notes

Village's culture doesn't affect settlement loyalty. Only castle and town culture does.

If you want a single village to have its own troops then you can change that village to a new culture. It won't affect the castle/town it's bound to.

Culture can exist without the female clan member (it was stated somewhere otherwise).

!!! danger "Culture XML file is very sensitive to the comments. Commenting out one name or one of basic_mercenary_troops will crash the game."

## XML

    spcultures.xml


is_main_culture - will this culture show in the new character creation / [culture selection menu](/guides/custom_culture_selection_screen/)?

    color="0xffC0C0C0"
    color2="0xffFFC5C5"

Colors define the armor colors for this culture's units in the Encyclopedia:

![](/pics/4LnIbLN.png)

These lines in define NPCs hirable in the Tavern, not the actual Caravan Guards that travel the world map.

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

These lines determine the equipment rosters for lords that turn 18:

    default_battle_equipment_roster="EquipmentRoster.___"
    default_civilian_equipment_roster="EquipmentRoster.___"


encounter_background_mesh - menu pic for encounter with the troops of this culture

### &lt;cultural_feats>

| Feat | Description |
| ------------ | --------- |
| empire_decreased_garrison_wage | <span style="color:green">20% less garrison troop wage</span> |
| empire_army_influence | <span style="color:green">Being in army brings 25% more influence</span> |
| empire_slower_hearth_production | <span style="color:red">Village hearths increase 20% less</span> |
| aserai_cheaper_caravans | <span style="color:green">Caravans are 30% cheaper to build. 10% less trade penalty</span> |
| aserai_desert_speed | <span style="color:green">No speed penalty on desert</span> |
| aserai_increased_wages | <span style="color:red">Daily wages of troops in the party are increased by 5%</span> |
| sturgian_cheaper_recruits_infantry | <span style="color:green">Recruiting and upgrading infantry troops are 25% cheaper</span> |
| sturgian_decreased_cohesion_rate | <span style="color:green">Armies lose 20% less daily cohesion</span> |
| sturgian_increased_decision_penalty | <span style="color:red">20% more relationship penalty from kingdom decisions</span> |
| vlandian_renown_mercenary_income | <span style="color:green">5% more renown from the battles, 15% more income while serving as a mercenary</span> |
| vlandian_villages_production_bonus | <span style="color:green">10% production bonus to villages that are bound to castles</span> |
| vlandian_increased_army_influence_cost | <span style="color:red">Recruiting lords to armies costs 20% more influence</span> |
| battanian_forest_speed | <span style="color:green">50% less speed penalty and 15% sight range bonus in forests</span> |
| battanian_militia_production | <span style="color:green">Towns owned by Battanian rulers have +1 militia production</span> |
| battanian_slower_construction | <span style="color:red">10% slower build rate for town projects in settlements</span> |
| khuzait_cheaper_recruits_mounted | <span style="color:green">Recruiting and upgrading mounted troops are 10% cheaper</span> |
| khuzait_increased_animal_production | <span style="color:green">25% production bonus to horse, mule, cow and sheep in villages owned by Khuzait rulers</span> |
| khuzait_decreased_town_tax | <span style="color:red">20% less tax income from towns</span> |


??? example "To change the description of the native feats, use Harmony patch (remove 'Battanian' example):"

    ``` cs
    [HarmonyPatch(typeof(DefaultCulturalFeats), "InitializeAll")]
    public static class InitializeAllPatch
    {
        static void Postfix(DefaultCulturalFeats __instance)
        {
            // Use reflection to access the private field _battaniaMilitiaFeat
            FieldInfo battaniaMilitiaFeatField = typeof(DefaultCulturalFeats).GetField("_battaniaMilitiaFeat", BindingFlags.NonPublic | BindingFlags.Instance);

            if (battaniaMilitiaFeatField != null)
            {
                // Get the current value of _battaniaMilitiaFeat
                var battaniaMilitiaFeat = battaniaMilitiaFeatField.GetValue(__instance) as FeatObject;

                if (battaniaMilitiaFeat != null)
                {
                    // Re-initialize _battaniaMilitiaFeat with the new string
                    battaniaMilitiaFeat.Initialize("{=!}battanian_militia_production", "{=lt_1qUFMK28}Towns owned by this culture's rulers have +1 militia production.", 1f, true, FeatObject.AdditionType.Add);
                }
            }
        }
    }
    ```

### &lt;possible_clan_banner_icon_ids>

These are the banner icons that the game will use when creating a banner for new clans or rebels.

![](/pics/banner_icons.png)


* [Creating Custom Banner Icons & Colors](https://moddocs.bannerlord.com/asset-management/creating_custom_banner_icons/)

### module_strings.xml

Related strings:

``` xml
  <string id="str_player_father_name.baltic" text="{=balticfather}Vaitas" />
  <string id="str_player_mother_name.baltic" text="{=balticmother}Laima" />
  <string id="str_neutral_term_for_culture.baltic" text="balts" />
  <string id="str_culture_description.baltic" text="{=balticdescription}The Balts, who live along the southeastern Baltic Sea, have their own languages, unique cultural practices, and pagan beliefs. They organize themselves into tribes, each led by a chieftain, maintaining independence while sharing a common cultural and religious bond." />
  <string id="str_faction_ruler_name_with_title.baltic" text="{=balticrulertitle}{RULER.NAME} {?RULER.GENDER}Queen{?}King{\?}" />
  <string id="str_faction_noble_name_with_title.baltic" text="{=balticnobletitle}{RULER.NAME} {?RULER.GENDER}Rikis{?}Rikis{\?}" />
  <string id="str_faction_official.baltic" text="{=balticofficial}a chieftain" />
  <string id="str_faction_official.baltic_f" text="{=balticofficialf}a lady" />
  <string id="str_faction_ruler.baltic" text="{=balticruler}King" />
  <string id="str_faction_ruler.baltic_f" text="{=balticrulerf}Queen" />
  <string id="str_faction_ruler_term_in_speech.baltic" text="{=balticrulerterm}{RULER.NAME} {?RULER.GENDER}Queen{?}King{\?}" />
  <string id="str_adjective_for_faction.baltic" text="{=balticfactionadjective}Baltic" />
  <string id="str_short_term_for_faction.baltic" text="{=balticfactionshortterm}Baltic" />
  <string id="str_faction_formal_name_for_culture.baltic" text="{=balticfactionformalname}The Baltic Tribes" />
  <string id="str_faction_informal_name_for_culture.baltic" text="{=balticfactioninformalname}the Baltic Tribes" />
  <string id="str_adjective_for_culture.baltic" text="{=balticcultureadjective}Baltic" />
  <string id="str_neutral_term_for_culture.baltic" text="{=balticcultureneutralterm}Baltic" />
  <string id="str_culture_rich_name.baltic" text="{=balticmembers}Baltic" />	
```




