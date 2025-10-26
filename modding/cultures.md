# Cultures

- [API 1.2.10](https://apidoc.bannerlord.com/v/1.2.10/class_tale_worlds_1_1_core_1_1_item_object.html)
- [XML Culture doc](https://docs.bannerlordmodding.com/_xmldocs/spcultures/culture/)
- [Custom Culture](/guides/custom_culture/)

## Default cultures

* aserai
* battania
* empire
* knuzait
* sturgia
* vlandia

and small other cultures, like looters, etc

## Check

    if (hero.Culture.StringId == "sturgia") ..


## Get All Cultures

``` cs
public class CultureHelper
{
    public static List<CultureObject> GetAllCultures()
    {
        if (Game.Current != null)
        {
            return Game.Current.ObjectManager.GetObjectTypeList<CultureObject>();
        }
        return new List<CultureObject>();
    }
}

// Usage
List<CultureObject> allCultures = CultureHelper.GetAllCultures();
foreach (var culture in allCultures)
{
    Console.WriteLine($"Culture ID: {culture.StringId}, Name: {culture.Name}");
}
```

## Menu backgrounds


* CULTURE_alley.png
* CULTURE_arena.png
* CULTURE_keep.png
* CULTURE_tavern.png


More [info](/guides/custom_menu_background/#alleyarenakeeptavern).

## Notes

Village's culture doesn't affect settlement loyalty. Only castle and town culture does.

If you want a single village to have its own troops then you can change that village to a new culture. It won't affect the castle/town it's bound to.

Culture can exist without the female clan member (it was stated somewhere otherwise).

!!! danger "Culture XML file is very sensitive to the comments. Commenting out one name or one of basic_mercenary_troops will crash the game."

Arena requires at least one tier 4/5 troop tree for a new culture. ([Iberian Wolf/fedeita](https://discord.com/channels/411286129317249035/411286129317249038/1231437906145443861))

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

??? abstract "1.2.12"
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

!!! abstract "1.3+"

| Feat | Description |
| ------------ | --------- |
| aserai_cheaper_caravans | <span style="color:green">Caravans are 30% cheaper to build. 10% less trade penalty</span> |
| aserai_desert_speed | <span style="color:green">No speed penalty on desert</span> |
| aserai_increased_wages | <span style="color:red">Daily wages of troops in the party are increased by 5%</span> |
| battanian_forest_speed | <span style="color:green">50% less speed penalty and 15% sight range bonus in forests</span> |
| battanian_militia_production | <span style="color:green">Towns owned by Battanian rulers have +1 militia production</span> |
| battanian_slower_construction | <span style="color:red">10% slower build rate for town projects in settlements</span> |
| empire_decreased_garrison_wage | <span style="color:green">20% less garrison troop wage</span> |
| empire_army_influence | <span style="color:green">Being in army brings 25% more influence</span> |
| empire_slower_hearth_production | <span style="color:red">Village hearths increase 20% less</span> |
| khuzait_cheaper_recruits_mounted | <span style="color:green">Recruiting and upgrading mounted troops are 10% cheaper</span> |
| khuzait_increased_animal_production | <span style="color:green">25% production bonus to horse, mule, cow and sheep in villages owned by Khuzait rulers</span> |
| khuzait_decreased_town_tax | <span style="color:red">20% less tax income from towns</span> |
| sturgian_increased_grain_production | :new: <span style="color:green">Villages grain production is increased by 10%</span> |
| sturgian_decreased_army_influence_cost | :new: <span style="color:green">Armies are gathered with 50% less influence</span> |
| sturgian_increased_decision_penalty | <span style="color:red">20% more relationship penalty from kingdom decisions</span> |
| vlandian_renown_mercenary_income | <span style="color:green">5% more renown from the battles, 15% more income while serving as a mercenary</span> |
| vlandian_villages_production_bonus | <span style="color:green">10% production bonus to villages that are bound to castles</span> |
| vlandian_increased_army_influence_cost | <span style="color:red">Recruiting lords to armies costs 20% more influence</span> |



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

### &lt;default_policies>

Can be set as &lt;policies> for [Kingdoms](/modding/kingdoms/#policies)

| Policy | Description |
| ------------ | --------- |
| policy_land_tax | 5% of the village income is paid to the ruler clan as tax. 5% less village income for clans |
| policy_state_monopolies | Ruler clan gains 5% of settlement as tax per town. Workshop production is decreased by 10% |
| policy_sacred_majesty | Ruler clan earns 3 influence per day. Non-ruler clans lose 0.5 influence per day |
| policy_magistrates | Town security is increased by 1 per day. Town taxes are reduced by 5% |
| policy_debasement_of_the_currency | Ruler clan gains 100 denars per day for each town in the kingdom. Settlement loyalty is decreased by 1 per day |
| policy_precarial_land_tenure | The influence cost of proposing settlement annexation is reduced by 50% for the ruler clan |
| policy_crown_duty | 5% tax on tariffs is paid to the ruler clan. Higher trade penalty in towns. Settlement prosperity is decreased by 1 per day |
| policy_imperial_towns | Towns held by the ruler clan gain 1 Loyalty and 1 Prosperity per day. Towns held by non-ruler clans lose 0.3 Loyalty per day |
| policy_royal_commissions | The influence cost of creating an army is reduced by 30% for the ruler. Armies led by the ruler earn cohesion at 30% less cost. Armies led by non-ruler nobles cost 10% more influence to create |
| policy_royal_guard | Ruler's party size is increased by 60. Non-ruling clans lose 0.2 influence per day |
| policy_war_tax | Ruler gains 5% tax from all settlements. Towns lose 1 prosperity per day. The influence cost of declaring war is doubled for the ruler clan |
| policy_royal_privilege | For kingdom decisions, the influence cost of the ruler overriding the popular decision outcome is reduced by 20% |
| policy_senate | Tier 3+ clans gain 0.5 influence per day, influence cost of inviting lower tier clans to army are increased by 10% |
| policy_lords_privy_council | Tier 5+ clans gain 0.5 influence per day, influence cost of inviting lower tier clans to army are increased by 20% |
| policy_military_coronae | Military achievements grant 20% more influence. Troop wages are increased by 10% |
| policy_feudal_inheritance | The cost of revoking a fief from a clan is doubled. Clans gain 0.1 influence for each fief they own |
| policy_serfdom | Villages grant 0.2 influence per day to the owner clan. Towns gain 1 security but lose 1 militia per day |
| policy_noble_retinues | Tier 5+ clans lose 1 influence per day and the party size of their leaders is increased by 40 |
| policy_castle_charters | Castle upgrade costs are reduced by 20% |
| policy_bailiffs | Town security is increased by 1 per day. Towns with a security greater than 60 yield 1 additional influence to the owner clan. Tax from towns are reduced by 5% |
| policy_hunting_rights | Food production in towns and castles are increased by 2. Town loyalty is decreased by 0.2 |
| policy_road_tolls | Trade tax paid to the town owner is increased by 3%. Town prosperity is decreased by 0.2 |
| policy_marshals | Armies led by Tier 5+ nobles require 10% less influence. Influence of the ruler clan is reduced by 1 per day |
| policy_council_of_the_commons | Each notable yields 0.1 influence per day to the settlement's owner clan. Tax from fortifications 5% decreased |
| policy_citizenship | Settlement loyalty is increased by 2 per day. Settlement production is reduced by 5% |
| policy_forgiveness_of_debts | 0.5 Loyalty per day to settlements that have the same culture as their owner clan. Settlement militia production is increased by 1. -0.5 Loyalty per day to settlements with a different culture than its owner clan |
| policy_tribunes_of_the_people | Town taxes paid to the ruler are reduced by 5%. Town loyalty is increased by 1 per day |
| policy_grazing_rights | Settlement loyalty is increased by 0.5 per day. Daily hearth production at villages decreases by 0.25 per day |
| policy_lawspeakers | All clans whose leader has high Charm gain 1 influence per day. All clans whose leader has low Charm lose 1 influence per day |
| policy_trial_by_jury | Settlement loyalty is increased by 0.5 per day. Settlement security is decreased by 0.2 per day. Clans lose 1 influence per day |
| policy_cantons | Daily militia production is increased by 1. Recruits replenish 20% faster. Tax income in settlements are reduced by 10% |
    Extracted by Alkoon


### &lt;possible_clan_banner_icon_ids>

These are the banner icons that the game will use when creating a banner for new clans or rebels.

![](/pics/banner_icons.png)

![](/pics/bannersASC.png)

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




