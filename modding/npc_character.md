# NPC Character

Defines NPC or template for a NPC

Set in SubModule.xml as:

``` xml
<XmlNode>
    <XmlName id="NPCCharacters" path="XML_FILE_OR_FOLDER"/>
    <IncludedGameTypes>
        <GameType value = "Campaign"/>
        <GameType value = "CampaignStoryMode"/>
        <GameType value = "CustomGame"/>
        <GameType value = "EditorGame"/>
    </IncludedGameTypes>
</XmlNode>
```

## NPCCharacter

- id
- is_template
- default_group
- formation_position_preference
- level
- name
- [voice](/modding/heroes/#voice)
- age (int, crash on float)
- is_female (default false)
- is_hero
- occupation
- is_basic_troop - it gets checked in the encyclopedia unit page for troops for determining the root of the troop tree and in the "Rival Gang Moving In" issue quest for determining what troops should spawn according to the mission difficulty
- culture
- is_hidden_encyclopedia
- skill_template

## face

face_key_template

The hairstyles and beards I want that particular troop to have: ([Source](https://discord.com/channels/411286129317249035/1231300826257948852/1259687116753866783) by Iberian Wolf)

``` xml
<face_key_template value="BodyProperty.young_fighter_xxx"/>
<hair_tags>
    <hair_tag name="Bald"/>
    <hair_tag name="Ponytail"/>
    <hair_tag name="LongAndBushy"/>
    <hair_tag name="TiedAcrossBack"/>
    <hair_tag name="SlickedLong"/>
    <hair_tag name="PartedAndCombedBack"/>
    <hair_tag name="Tousled"/>
    <hair_tag name="Short"/>
    <hair_tag name="HighPonytail"/>
    <hair_tag name="Bowl"/>
    <hair_tag name="ShortAndThin"/>
    <hair_tag name="SlickedShort"/>
    <hair_tag name="ShortWavy"/>
    <hair_tag name="Bandit Hair 1"/>
    <hair_tag name="Bandit Hair 2"/>
    <hair_tag name="Bandit Hair 3"/>
    <hair_tag name="Afro Hair 4"/>
    <hair_tag name="Afro Hair 5"/>
    <hair_tag name="Monk Hair"/>
</hair_tags>
<beard_tags>
    <beard_tag name="Cleanshaven"/>
    <beard_tag name="MustacheAndPatch"/>
    <beard_tag name="MediumMustache"/>
    <beard_tag name="BeardWithShavedCheeks"/>
    <beard_tag name="TrimmedDutch"/>
    <beard_tag name="MediumDutch"/>
    <beard_tag name="LightShortBeard"/>
    <beard_tag name="HeavyShortBeard"/>
    <beard_tag name="LongBeard"/>
</beard_tags>
```

## upgrade_targets

## skills

## Traits

## Equipments

EquipmentSet
