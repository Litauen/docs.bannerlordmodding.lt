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
