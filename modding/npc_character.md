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
- is_basic_troop (true if this troop should start the troop chain in the Encyclopedia)
- culture
- is_hidden_encyclopedia
- skill_template

## face

face_key_template

## upgrade_targets

## skills

## Traits

## Equipments

EquipmentSet
