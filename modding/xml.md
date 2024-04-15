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



## Formatting

??? success "Make ugly XMLs easier to work with!"
    ![](/pics/xENrxdr.png)

??? hint "Notepad++ with XML Tools plugin"
    ![](/pics/M6QGjrP.png)

Online tools:

- [Make it readable](https://www.liquid-technologies.com/online-xml-formatter){target=_blank}
- [Fix Tab spaces](https://jsonformatter.org/xml-formatter){target=_blank}



## XSLT

Use XSLT files to remove or modify the elements of XML files that are loaded from other modules. You don’t have to use the XSLT system for adding new elements to the XML files.

You simply create an XSLT file with the same name as the XML file and place it to the same path with your XML file. This added XSLT file is going to make the changes to the XML files of the same type located in modules that were loaded before your module. 

??? warning "In order XSLT to be applied, there should be a XML file with the same name in the same folder.<br>XML could be empty."
    ![](/pics/2402171157.png)

The system applies your module’s XSLT files just before applying your module’s own XML files. So the XSLT files are not going to affect the same module’s own XML files.

* [XSLT Usage Tutorial](https://moddocs.bannerlord.com/bestpractices/xslt_usage_tutorial/){target=_blank}
* [Validation](https://www.freeformatter.com/xsl-transformer.html){target=_blank}
* [Update Attribute in Elements](https://gist.github.com/cpburnz/e11fa0b792e81ee071d443b64e06516f){target=_blank}
* [Remove Attribute from Elements](https://gist.github.com/cpburnz/6cc05c4a0ea4d66e875ccbebbd6eda4a){target=_blank}


### Delete an entry

``` xml
<xsl:template match="Faction[@id='clan_vlandia_11']"/>
```

### Change an attribute

``` xml
<xsl:template match="Faction[@id='clan_sturgia_2']/@tier">
    <xsl:attribute name="tier">5</xsl:attribute>
</xsl:template>

```

### Add new attribute

``` xml
<xsl:template match="NPCCharacter[@id='aserai_recruit']">
    <xsl:copy>
        <!-- Copy existing attributes -->
        <xsl:apply-templates select="@*"/>
        <!-- Add the new attribute -->
        <xsl:attribute name="is_hidden_encyclopedia">true</xsl:attribute>
        <!-- Process child nodes -->
        <xsl:apply-templates select="node()"/>
    </xsl:copy>
</xsl:template>
```

### Change a section

``` xml
<xsl:template match="Kingdom[@id='aserai']/relationships">
    <relationships>
        <relationship kingdom="Kingdom.empire_s" value="-1" isAtWar="true" />
    </relationships>
</xsl:template>
```

### Match several elements at once

``` xml
<xsl:template match="NPCCharacter[
    @id='caravan_master_aserai' or
    @id='caravan_master_battania' or
    @id='khuzait_militia_veteran_spearman']">
    <xsl:copy>
        <xsl:apply-templates select="@*"/>
        <xsl:attribute name="is_hidden_encyclopedia">true</xsl:attribute>
        <xsl:apply-templates select="node()"/>
    </xsl:copy>
</xsl:template>

```


### Example

Will change Caladog

In SubModule.xml:

``` xml
<XmlNode>
    <XmlName id="NPCCharacters" path="npc_lords"/>
    <IncludedGameTypes>
        <GameType value = "Campaign"/>
        <GameType value = "CampaignStoryMode"/>
        <GameType value = "CustomGame"/>
        <GameType value = "EditorGame"/>
    </IncludedGameTypes>
</XmlNode>
```

In your Module data create folder **npc_lords**

Add lords.xml file there:

``` xml
<?xml version="1.0" encoding="utf-8"?>
<NPCCharacters>
</NPCCharacters>
```

??? code "Add lords.xslt file"

    ``` xml
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output omit-xml-declaration="yes"/>
        <xsl:template match="@*|node()">
            <xsl:copy>
                <xsl:apply-templates select="@*|node()"/>
            </xsl:copy>
        </xsl:template>

        <xsl:template match="NPCCharacter[@id='lord_5_1']">
            <NPCCharacter id="lord_5_1" name="Alibaba" voice="earnest" age="42" is_female="false" is_hero="true" culture="Culture.aserai" occupation="Lord" default_group="Cavalry">
                <face>
                    <!-- custom face -->
                    <BodyProperties version="4" age="42.27" weight="0.3787" build="0.9598" key="000DA40980001948AF7B0F8C4F65FFE877F685FF6F72FA04C52A54008AF25F986E7006350B469080E60034000040F124000000000000003B0000000005743086"/>
                </face>
                <skills>
                    <skill id="Engineering" value="170"/>
                    <skill id="Medicine" value="71"/>
                    <skill id="Leadership" value="250"/>
                    <skill id="Steward" value="53"/>
                    <skill id="Trade" value="199"/>
                    <skill id="Charm" value="150"/>
                    <skill id="Roguery" value="66"/>
                    <skill id="Scouting" value="121"/>
                    <skill id="Tactics" value="200"/>
                    <skill id="Crafting" value="131"/>
                    <skill id="Athletics" value="240"/>
                    <skill id="Riding" value="200"/>
                    <skill id="Throwing" value="4"/>
                    <skill id="Crossbow" value="81"/>
                    <skill id="Bow" value="169"/>
                    <skill id="Polearm" value="200"/>
                    <skill id="TwoHanded" value="200"/>
                    <skill id="OneHanded" value="200"/>
                </skills>
                <Traits>
                    <Trait id="Mercy" value="-1"/>
                    <Trait id="Valor" value="0"/>
                    <Trait id="Honor" value="-1"/>
                    <Trait id="Generosity" value="1"/>
                    <Trait id="Calculating" value="2"/>
                    <Trait id="Egalitarian" value="-1"/>
                    <Trait id="Oligarchic" value="0"/>
                    <Trait id="Authoritarian" value="0"/>
                </Traits>
                <Equipments>
                    <EquipmentSet id="bat_king_template_bat_m"/>
                    <EquipmentSet id="bat_king_template_civ_m" civilian="true"/>
                </Equipments>
            </NPCCharacter>
        </xsl:template>

    </xsl:stylesheet>
    ```
