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




### Town menu picture

New culture requires new town menu sprite, without it it's empty:

![](/pics/NsKHhOg.png)