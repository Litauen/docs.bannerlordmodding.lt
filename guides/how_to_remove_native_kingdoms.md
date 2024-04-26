# How to remove native kingdoms

This should be done in steps, testing after each step.


This will come in handy: [🗲CRASHES🗲](/modding/crashes)


For example let's remove Aserai Kingdom.



## Harmony Patch

Game expects some heroes and settlements and looks for them using their IDs. Let's remove this with:

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

Compile this into your module, make sure it's loaded when you test new game.

Without this, game will crash on start if any of these IDs are removed.



## settlements.xml

!!! danger "Make a full backup of your /Modules/YOUR_MOD folder"

You have 2 choices here: keep native settlements or remove them.

### Transfer Towns/Castles to other Kingdoms

Assign all kingdom's towns and castles to other kingdoms by changing the owner to different clan:

```xml
<Settlement id="castle_A6" name="{=Settlements.Settlement.name.castle_A6}Shibal Zumr Castle" owner="Faction.clan_aserai_3" posX="666.93" posY="245.247" culture="Culture.aserai" gate_posX="665.921" gate_posY="246.1539">
```

``` xml
owner="Faction.clan_aserai_3"
```

should be some other clan, let's say Faction.clan_empire_north_1

Do this to all towns/castles, do not change the capital, keep it to the current kingdom's ruler, in our case: clan_aserai_1 / Quyaz town_A1

!!! warning "Make sure you have assigned ruling clan clan..._1 to the capital"

No changes to the villages are necessary, they are assigned to the towns/castles and will move to the new clans automatically.


### Delete native Settlements

If you want to get rid of native settlements:

Comment out all towns, castles AND village except Capital, like this:

``` xml
<!--
  <Settlement id="castle_A6" name="{=Settlements.Settlement.name.castle_A6}Shibal Zumr Castle" owner="Faction.clan_aserai_3" posX="666.93" posY="245.247" culture="Culture.aserai" gate_posX="665.921" gate_posY="246.1539">
    <Components>
      <Town id="castle_comp_A6" is_castle="true" level="1" background_crop_position="0.0" background_mesh="menu_aserai_3" wait_mesh="wait_aserai_town" gate_rotation="0.908" prosperity="570" />
    </Components>
    <Locations complex_template="LocationComplexTemplate.castle_complex">
      <Location id="center" scene_name="aserai_castle_002" scene_name_1="aserai_castle_002" scene_name_2="aserai_castle_002" scene_name_3="aserai_castle_002" />
      <Location id="lordshall" scene_name_1="aserai_castle_keep_a_l1_interior" scene_name_2="aserai_castle_keep_a_l2_interior" scene_name_3="aserai_castle_keep_a_l3_interior" />
      <Location id="prison" scene_name="aserai_dungeon_a" />
    </Locations>
  </Settlement>
-->
```

!!! warning "Make sure you have assigned ruling clan clan..._1 to the capital"

### Test->Backup

Run a NEW game, it should not crash. If you assigned settlements to another kingdom, they should belong to another kingdom if you removed settlements, the map will show inactive settlements, you will not be able to enter them.

Capital should belong to the ruling clan. All other clans will run around the capital.

If all works:

!!! danger "Make a full backup of your /Modules/YOUR_MOD folder under a new name"


## Heroes/Lords/Clans

Remove all clans/lords/heroes except clan_1 and it's ruler and his wife.

!!! failure "I leave ruler and the spouse because I was not able to remove them without crashes. Later we will rename them to other leaders/clans keeping their native IDs."

### heroes.xslt


??? example "Removing all native Aserai heroes:"

    ``` xml
    <xsl:template match="Hero[@id='dead_lord_3_1']"/>
    <xsl:template match="Hero[@id='lord_3_7']"/>
    <xsl:template match="Hero[@id='lord_3_10']"/>
    <xsl:template match="Hero[@id='lord_3_13']"/>
    <xsl:template match="Hero[@id='lord_3_13_1']"/>
    <xsl:template match="Hero[@id='lord_3_13_2']"/>
    <xsl:template match="Hero[@id='lord_3_8']"/>
    <xsl:template match="Hero[@id='lord_3_11']"/>
    <xsl:template match="Hero[@id='lord_3_3']"/>
    <xsl:template match="Hero[@id='lord_3_3_1']"/>
    <xsl:template match="Hero[@id='lord_3_4']"/>
    <xsl:template match="Hero[@id='lord_3_5']"/>
    <xsl:template match="Hero[@id='lord_3_51']"/>
    <xsl:template match="Hero[@id='lord_3_12']"/>
    <xsl:template match="Hero[@id='lord_3_20']"/>
    <xsl:template match="Hero[@id='lord_3_20_1']"/>
    <xsl:template match="Hero[@id='lord_3_20_2']"/>
    <xsl:template match="Hero[@id='lord_3_6']"/>
    <xsl:template match="Hero[@id='lord_3_9']"/>
    <xsl:template match="Hero[@id='lord_3_14']"/>
    <xsl:template match="Hero[@id='lord_3_14_1']"/>
    <xsl:template match="Hero[@id='lord_3_15']"/>
    <xsl:template match="Hero[@id='lord_3_15_1']"/>
    <xsl:template match="Hero[@id='lord_3_15_2']"/>
    <xsl:template match="Hero[@id='lord_3_16']"/>
    <xsl:template match="Hero[@id='lord_3_16_1']"/>
    <xsl:template match="Hero[@id='lord_3_17']"/>
    <xsl:template match="Hero[@id='lord_3_17_1']"/>
    <xsl:template match="Hero[@id='lord_3_17_2']"/>
    <xsl:template match="Hero[@id='lord_3_21']"/>
    <xsl:template match="Hero[@id='lord_3_21_1']"/>
    <xsl:template match="Hero[@id='lord_3_18']"/>
    <xsl:template match="Hero[@id='lord_3_18_1']"/>
    <xsl:template match="Hero[@id='lord_3_18_2']"/>
    <xsl:template match="Hero[@id='lord_3_18_3']"/>
    <xsl:template match="Hero[@id='lord_3_18_4']"/>
    <xsl:template match="Hero[@id='lord_3_19']"/>
    <xsl:template match="Hero[@id='lord_3_19_1']"/>
    <xsl:template match="Hero[@id='lord_3_19_2']"/>
    <xsl:template match="Hero[@id='lord_3_19_3']"/>
    <xsl:template match="Hero[@id='lord_3_23']"/>
    <xsl:template match="Hero[@id='lord_3_23_1']"/>
    <xsl:template match="Hero[@id='lord_3_22']"/>
    <xsl:template match="Hero[@id='lord_3_22_1']"/>
    <xsl:template match="Hero[@id='lord_3_22_2']"/>
    <xsl:template match="Hero[@id='lord_3_22_3']"/>
    <xsl:template match="Hero[@id='lord_3_22_4']"/>
    <xsl:template match="Hero[@id='lord_A9_l']"/>
    <xsl:template match="Hero[@id='lord_A9_s']"/>
    <xsl:template match="Hero[@id='lord_A9_c']"/>
    <xsl:template match="Hero[@id='lord_A9_u']"/>
    ```

Leaving lord_3_1 (ruler) and lord_3_2 (his wife) intact.

Examples for other Kingdoms:

??? example "Vlandia:"
    ``` xml
    <xsl:template match="Hero[@id='lord_4_7']"/>
    <xsl:template match="Hero[@id='lord_4_10']"/>
    <xsl:template match="Hero[@id='lord_4_13']"/>
    <xsl:template match="Hero[@id='lord_4_14']"/>
    <xsl:template match="Hero[@id='lord_4_141']"/>
    <xsl:template match="Hero[@id='lord_4_15']"/>
    <xsl:template match="Hero[@id='lord_4_3']"/>
    <xsl:template match="Hero[@id='lord_4_3_1']"/>
    <xsl:template match="Hero[@id='lord_4_4']"/>
    <xsl:template match="Hero[@id='lord_4_8']"/>
    <xsl:template match="Hero[@id='lord_4_11']"/>
    <xsl:template match="Hero[@id='lord_4_6']"/>
    <xsl:template match="Hero[@id='lord_4_6_1']"/>
    <xsl:template match="Hero[@id='lord_4_5']"/>
    <xsl:template match="Hero[@id='lord_4_9']"/>
    <xsl:template match="Hero[@id='lord_4_12']"/>
    <xsl:template match="Hero[@id='lord_4_121']"/>
    <xsl:template match="Hero[@id='lord_4_16']"/>
    <xsl:template match="Hero[@id='lord_4_16_1']"/>
    <xsl:template match="Hero[@id='lord_4_17']"/>
    <xsl:template match="Hero[@id='lord_4_18']"/>
    <xsl:template match="Hero[@id='lord_4_181']"/>
    <xsl:template match="Hero[@id='lord_4_19']"/>
    <xsl:template match="Hero[@id='lord_4_25']"/>
    <xsl:template match="Hero[@id='lord_4_25_1']"/>
    <xsl:template match="Hero[@id='lord_4_21']"/>
    <xsl:template match="Hero[@id='lord_4_20']"/>
    <xsl:template match="Hero[@id='lord_4_20_1']"/>
    <xsl:template match="Hero[@id='lord_4_22']"/>
    <xsl:template match="Hero[@id='lord_4_22_1']"/>
    <xsl:template match="Hero[@id='lord_4_23']"/>
    <xsl:template match="Hero[@id='lord_4_23_1']"/>
    <xsl:template match="Hero[@id='lord_4_23_2']"/>
    <xsl:template match="Hero[@id='lord_4_23_3']"/>
    <xsl:template match="Hero[@id='lord_4_24']"/>
    <xsl:template match="Hero[@id='lord_4_24_1']"/>
    <xsl:template match="Hero[@id='lord_4_24_2']"/>
    <xsl:template match="Hero[@id='lord_4_24_3']"/>
    <xsl:template match="Hero[@id='lord_4_24_4']"/>
    <xsl:template match="Hero[@id='lord_4_26']"/>
    <xsl:template match="Hero[@id='lord_4_26_1']"/>
    <xsl:template match="Hero[@id='lord_4_27']"/>
    <xsl:template match="Hero[@id='lord_4_27_1']"/>
    <xsl:template match="Hero[@id='lord_V9_u']"/>
    <xsl:template match="Hero[@id='lord_4_28']"/>
    <xsl:template match="Hero[@id='lord_4_28_1']"/>
    <xsl:template match="Hero[@id='lord_4_28_2']"/>
    <xsl:template match="Hero[@id='lord_V11_l']"/>
    <xsl:template match="Hero[@id='lord_V11_u']"/>
    <xsl:template match="Hero[@id='lord_V11_c1']"/>
    <xsl:template match="Hero[@id='lord_V11_c2']"/>

    Leave: lord_4_1 and lord_4_2
    ```

??? example "Khuzait:"
    ``` xml
    <xsl:template match="Hero[@id='dead_lord_6_1']"/>
    <xsl:template match="Hero[@id='lord_6_7']"/>
    <xsl:template match="Hero[@id='lord_6_10']"/>
    <xsl:template match="Hero[@id='lord_6_101']"/>
    <xsl:template match="Hero[@id='lord_6_13']"/>
    <xsl:template match="Hero[@id='dead_lord_6_2']"/>
    <xsl:template match="Hero[@id='lord_6_3']"/>
    <xsl:template match="Hero[@id='lord_6_4']"/>
    <xsl:template match="Hero[@id='lord_6_8']"/>
    <xsl:template match="Hero[@id='lord_6_81']"/>
    <xsl:template match="Hero[@id='lord_6_11']"/>
    <xsl:template match="Hero[@id='dead_lord_6_3']"/>
    <xsl:template match="Hero[@id='dead_lord_6_4']"/>
    <xsl:template match="Hero[@id='lord_6_5']"/>
    <xsl:template match="Hero[@id='lord_6_51']"/>
    <xsl:template match="Hero[@id='lord_6_9']"/>
    <xsl:template match="Hero[@id='lord_6_12']"/>
    <xsl:template match="Hero[@id='lord_6_15']"/>
    <xsl:template match="Hero[@id='lord_6_15_1']"/>
    <xsl:template match="Hero[@id='lord_6_15_2']"/>
    <xsl:template match="Hero[@id='lord_6_6']"/>
    <xsl:template match="Hero[@id='lord_6_16']"/>
    <xsl:template match="Hero[@id='lord_6_16_1']"/>
    <xsl:template match="Hero[@id='lord_6_16_2']"/>
    <xsl:template match="Hero[@id='lord_6_24']"/>
    <xsl:template match="Hero[@id='lord_6_17']"/>
    <xsl:template match="Hero[@id='lord_6_17_1']"/>
    <xsl:template match="Hero[@id='lord_6_17_2']"/>
    <xsl:template match="Hero[@id='lord_6_21']"/>
    <xsl:template match="Hero[@id='lord_6_21_1']"/>
    <xsl:template match="Hero[@id='lord_6_18']"/>
    <xsl:template match="Hero[@id='lord_6_18_1']"/>
    <xsl:template match="Hero[@id='lord_6_18_2']"/>
    <xsl:template match="Hero[@id='lord_6_22']"/>
    <xsl:template match="Hero[@id='lord_6_22_1']"/>
    <xsl:template match="Hero[@id='lord_6_19']"/>
    <xsl:template match="Hero[@id='lord_6_19_1']"/>
    <xsl:template match="Hero[@id='lord_6_19_2']"/>
    <xsl:template match="Hero[@id='lord_6_23']"/>
    <xsl:template match="Hero[@id='lord_6_20']"/>
    <xsl:template match="Hero[@id='lord_6_20_1']"/>
    <xsl:template match="Hero[@id='lord_K8_u']"/>
    <xsl:template match="Hero[@id='lord_K9_l']"/>
    <xsl:template match="Hero[@id='lord_K9_s']"/>
    <xsl:template match="Hero[@id='lord_K9_c1']"/>
    <xsl:template match="Hero[@id='lord_K9_c2']"/>

    Leave lord_6_1 and lord_6_2
    ```

??? example "Battania:"
    ``` xml
    <xsl:template match="Hero[@id='lord_5_3_1']"/>
    <xsl:template match="Hero[@id='lord_5_4']"/>
    <xsl:template match="Hero[@id='lord_5_5']"/>
    <xsl:template match="Hero[@id='lord_5_6']"/>
    <xsl:template match="Hero[@id='lord_5_19']"/>
    <xsl:template match="Hero[@id='lord_5_14']"/>
    <xsl:template match="Hero[@id='lord_5_14_1']"/>
    <xsl:template match="Hero[@id='lord_5_22']"/>
    <xsl:template match="Hero[@id='lord_5_16']"/>
    <xsl:template match="Hero[@id='lord_5_16_1']"/>
    <xsl:template match="Hero[@id='lord_5_17']"/>
    <xsl:template match="Hero[@id='lord_5_17_1']"/>
    <xsl:template match="Hero[@id='lord_B8_l']"/>
    <xsl:template match="Hero[@id='lord_5_1_1']"/>
    <xsl:template match="Hero[@id='lord_5_3']"/>
    <xsl:template match="Hero[@id='lord_5_3_2']"/>
    <xsl:template match="Hero[@id='lord_5_5_1']"/>
    <xsl:template match="Hero[@id='lord_5_7']"/>
    <xsl:template match="Hero[@id='lord_5_8']"/>
    <xsl:template match="Hero[@id='lord_5_9']"/>
    <xsl:template match="Hero[@id='lord_5_91']"/>
    <xsl:template match="Hero[@id='lord_5_10']"/>
    <xsl:template match="Hero[@id='lord_5_11']"/>
    <xsl:template match="Hero[@id='lord_5_12']"/>
    <xsl:template match="Hero[@id='lord_5_13']"/>
    <xsl:template match="Hero[@id='lord_5_13_1']"/>
    <xsl:template match="Hero[@id='lord_5_131']"/>
    <xsl:template match="Hero[@id='lord_5_15']"/>
    <xsl:template match="Hero[@id='lord_5_15_1']"/>
    <xsl:template match="Hero[@id='lord_5_15_2']"/>
    <xsl:template match="Hero[@id='lord_5_15_3']"/>
    <xsl:template match="Hero[@id='lord_5_16_2']"/>
    <xsl:template match="Hero[@id='lord_5_18']"/>
    <xsl:template match="Hero[@id='lord_5_18_1']"/>
    <xsl:template match="Hero[@id='lord_5_20']"/>
    <xsl:template match="Hero[@id='lord_5_21']"/>
    <xsl:template match="Hero[@id='lord_5_21_1']"/>
    <xsl:template match="Hero[@id='lord_5_21_2']"/>
    <xsl:template match="Hero[@id='lord_B8_s']"/>
    <xsl:template match="Hero[@id='lord_B8_c']"/>

    Leave: lord_5_1 (has no wife)
    ```

??? example "Sturgia:"
    ``` xml
    <xsl:template match="Hero[@id='dead_lord_2_1']"/>
    <xsl:template match="Hero[@id='dead_lord_2_2']"/>
    <xsl:template match="Hero[@id='lord_2_13_1']"/>
    <xsl:template match="Hero[@id='lord_2_13_2']"/>
    <xsl:template match="Hero[@id='lord_2_13_3']"/>
    <xsl:template match="Hero[@id='lord_2_13_4']"/>
    <xsl:template match="Hero[@id='lord_2_7']"/>
    <xsl:template match="Hero[@id='lord_2_7_1']"/>
    <xsl:template match="Hero[@id='lord_2_10']"/>
    <xsl:template match="Hero[@id='lord_2_13']"/>
    <xsl:template match="Hero[@id='lord_2_3']"/>
    <xsl:template match="Hero[@id='lord_2_4']"/>
    <xsl:template match="Hero[@id='lord_2_4_1']"/>
    <xsl:template match="Hero[@id='lord_2_8']"/>
    <xsl:template match="Hero[@id='lord_2_11']"/>
    <xsl:template match="Hero[@id='lord_2_111']"/>
    <xsl:template match="Hero[@id='lord_2_5']"/>
    <xsl:template match="Hero[@id='lord_2_6']"/>
    <xsl:template match="Hero[@id='lord_2_9']"/>
    <xsl:template match="Hero[@id='lord_2_12']"/>
    <xsl:template match="Hero[@id='lord_2_14']"/>
    <xsl:template match="Hero[@id='lord_2_121']"/>
    <xsl:template match="Hero[@id='lord_2_14_1']"/>
    <xsl:template match="Hero[@id='lord_2_14_2']"/>
    <xsl:template match="Hero[@id='lord_2_14_3']"/>
    <xsl:template match="Hero[@id='lord_2_16']"/>
    <xsl:template match="Hero[@id='lord_2_16_1']"/>
    <xsl:template match="Hero[@id='lord_2_21']"/>
    <xsl:template match="Hero[@id='lord_2_21_1']"/>
    <xsl:template match="Hero[@id='lord_2_17']"/>
    <xsl:template match="Hero[@id='lord_2_17_1']"/>
    <xsl:template match="Hero[@id='lord_2_22']"/>
    <xsl:template match="Hero[@id='lord_2_22_1']"/>
    <xsl:template match="Hero[@id='lord_2_24']"/>
    <xsl:template match="Hero[@id='lord_2_24_1']"/>
    <xsl:template match="Hero[@id='lord_2_18']"/>
    <xsl:template match="Hero[@id='lord_2_18_1']"/>
    <xsl:template match="Hero[@id='lord_2_23']"/>
    <xsl:template match="Hero[@id='lord_2_23_1']"/>
    <xsl:template match="Hero[@id='lord_2_15']"/>
    <xsl:template match="Hero[@id='lord_2_15_1']"/>
    <xsl:template match="Hero[@id='lord_2_15_2']"/>
    <xsl:template match="Hero[@id='lord_2_15_3']"/>
    <xsl:template match="Hero[@id='lord_2_15_4']"/>
    <xsl:template match="Hero[@id='lord_2_19']"/>
    <xsl:template match="Hero[@id='lord_2_19_1']"/>
    <xsl:template match="Hero[@id='lord_2_20']"/>
    <xsl:template match="Hero[@id='lord_2_20_1']"/>
    <xsl:template match="Hero[@id='lord_S8_u']"/>
    <xsl:template match="Hero[@id='lord_S9_l']"/>
    <xsl:template match="Hero[@id='lord_S9_m']"/>
    <xsl:template match="Hero[@id='lord_S9_c']"/>
    <xsl:template match="Hero[@id='lord_S9_u']"/>

    Leave: lord_2_1 and lord_2_2
    ```



??? example "Empire South:"
    ``` xml
    <xsl:template match="Hero[@id='lord_1_27']"/>
    <xsl:template match="Hero[@id='lord_1_27_1']"/>
    <xsl:template match="Hero[@id='lord_1_27_2']"/>
    <xsl:template match="Hero[@id='lord_1_27_3']"/>
    <xsl:template match="Hero[@id='lord_1_37']"/>
    <xsl:template match="Hero[@id='lord_1_47']"/>
    <xsl:template match="Hero[@id='lord_1_47_1']"/>
    <xsl:template match="Hero[@id='lord_1_47_2']"/>
    <xsl:template match="Hero[@id='lord_1_47_3']"/>
    <xsl:template match="Hero[@id='lord_1_15']"/>
    <xsl:template match="Hero[@id='lord_1_155']"/>
    <xsl:template match="Hero[@id='lord_1_16']"/>
    <xsl:template match="Hero[@id='lord_1_28']"/>
    <xsl:template match="Hero[@id='lord_1_38']"/>
    <xsl:template match="Hero[@id='lord_1_48']"/>
    <xsl:template match="Hero[@id='lord_1_48_1']"/>
    <xsl:template match="Hero[@id='lord_1_48_2']"/>
    <xsl:template match="Hero[@id='lord_1_48_3']"/>
    <xsl:template match="Hero[@id='lord_1_177']"/>
    <xsl:template match="Hero[@id='lord_1_29']"/>
    <xsl:template match="Hero[@id='lord_1_17']"/>
    <xsl:template match="Hero[@id='lord_1_18']"/>
    <xsl:template match="Hero[@id='lord_1_39']"/>
    <xsl:template match="Hero[@id='lord_1_30']"/>
    <xsl:template match="Hero[@id='lord_1_30_1']"/>
    <xsl:template match="Hero[@id='lord_1_49']"/>
    <xsl:template match="Hero[@id='lord_1_49_1']"/>
    <xsl:template match="Hero[@id='lord_1_49_2']"/>
    <xsl:template match="Hero[@id='lord_1_30_2']"/>
    <xsl:template match="Hero[@id='lord_1_30_3']"/>
    <xsl:template match="Hero[@id='lord_1_56_2']"/>
    <xsl:template match="Hero[@id='lord_1_63_2']"/>
    <xsl:template match="Hero[@id='lord_1_63_3']"/>
    <xsl:template match="Hero[@id='lord_1_63']"/>
    <xsl:template match="Hero[@id='lord_1_63_1']"/>
    <xsl:template match="Hero[@id='lord_1_74']"/>
    <xsl:template match="Hero[@id='lord_1_74_1']"/>
    <xsl:template match="Hero[@id='lord_1_54']"/>
    <xsl:template match="Hero[@id='lord_1_54_1']"/>
    <xsl:template match="Hero[@id='lord_1_68']"/>
    <xsl:template match="Hero[@id='lord_1_68_1']"/>
    <xsl:template match="Hero[@id='lord_1_69_2']"/>
    <xsl:template match="Hero[@id='lord_1_69']"/>
    <xsl:template match="Hero[@id='lord_1_69_1']"/>
    <xsl:template match="Hero[@id='lord_1_55']"/>
    <xsl:template match="Hero[@id='lord_1_55_1']"/>
    <xsl:template match="Hero[@id='lord_1_72']"/>
    <xsl:template match="Hero[@id='lord_1_72_1']"/>
    <xsl:template match="Hero[@id='lord_SE8_c']"/>
    <xsl:template match="Hero[@id='lord_SE9_l']"/>
    <xsl:template match="Hero[@id='lord_SE9_s']"/>
    <xsl:template match="Hero[@id='lord_SE9_c1']"/>
    <xsl:template match="Hero[@id='lord_SE9_c2']"/>

    Leave: lord_1_14 (no spouse)
    ```

??? example "Empire North:"
    ``` xml
    <xsl:template match="Hero[@id='lord_1_20']"/>
    <xsl:template match="Hero[@id='lord_1_41']"/>
    <xsl:template match="Hero[@id='lord_1_411']"/>
    <xsl:template match="Hero[@id='lord_1_31']"/>
    <xsl:template match="Hero[@id='lord_1_1_1']"/>
    <xsl:template match="Hero[@id='lord_1_1_2']"/>
    <xsl:template match="Hero[@id='lord_1_21']"/>
    <xsl:template match="Hero[@id='lord_1_3']"/>
    <xsl:template match="Hero[@id='lord_1_4']"/>
    <xsl:template match="Hero[@id='lord_1_22']"/>
    <xsl:template match="Hero[@id='lord_1_42']"/>
    <xsl:template match="Hero[@id='lord_1_32']"/>
    <xsl:template match="Hero[@id='lord_1_422']"/>
    <xsl:template match="Hero[@id='lord_1_1_3']"/>
    <xsl:template match="Hero[@id='lord_1_1_4']"/>
    <xsl:template match="Hero[@id='lord_1_1_5']"/>
    <xsl:template match="Hero[@id='lord_1_1_6']"/>
    <xsl:template match="Hero[@id='lord_1_5']"/>
    <xsl:template match="Hero[@id='lord_1_1_7']"/>
    <xsl:template match="Hero[@id='lord_1_1_8']"/>
    <xsl:template match="Hero[@id='lord_1_6']"/>
    <xsl:template match="Hero[@id='lord_1_33']"/>
    <xsl:template match="Hero[@id='lord_1_43']"/>
    <xsl:template match="Hero[@id='lord_1_1_9']"/>
    <xsl:template match="Hero[@id='lord_1_1_10']"/>
    <xsl:template match="Hero[@id='lord_1_1_11']"/>
    <xsl:template match="Hero[@id='lord_1_1_12']"/>
    <xsl:template match="Hero[@id='lord_1_1_13']"/>
    <xsl:template match="Hero[@id='lord_1_1_14']"/>
    <xsl:template match="Hero[@id='lord_1_64']"/>
    <xsl:template match="Hero[@id='lord_1_1_17']"/>
    <xsl:template match="Hero[@id='lord_1_50']"/>
    <xsl:template match="Hero[@id='lord_1_66']"/>
    <xsl:template match="Hero[@id='lord_1_1_15']"/>
    <xsl:template match="Hero[@id='lord_1_1_16']"/>
    <xsl:template match="Hero[@id='lord_1_51']"/>
    <xsl:template match="Hero[@id='lord_1_67']"/>
    <xsl:template match="Hero[@id='lord_1_58']"/>
    <xsl:template match="Hero[@id='lord_1_70']"/>
    <xsl:template match="Hero[@id='lord_NE7_u']"/>
    <xsl:template match="Hero[@id='lord_NE8_l']"/>
    <xsl:template match="Hero[@id='lord_NE8_s']"/>
    <xsl:template match="Hero[@id='lord_NE8_c1']"/>
    <xsl:template match="Hero[@id='lord_NE8_c2']"/>
    <xsl:template match="Hero[@id='lord_1_56']"/>
    <xsl:template match="Hero[@id='lord_1_56_1']"/>
    <xsl:template match="Hero[@id='lord_NE9_l']"/>
    <xsl:template match="Hero[@id='lord_NE9_s']"/>
    <xsl:template match="Hero[@id='lord_NE9_d']"/>

    Leave: lord_1_1 and lord_1_2
    ```

??? example "Empire West:"
    ``` xml
    <xsl:template match="Hero[@id='lord_1_75']"/>
    <xsl:template match="Hero[@id='lord_1_34']"/>
    <xsl:template match="Hero[@id='lord_1_24']"/>
    <xsl:template match="Hero[@id='lord_1_44']"/>
    <xsl:template match="Hero[@id='lord_1_9']"/>
    <xsl:template match="Hero[@id='lord_1_10']"/>
    <xsl:template match="Hero[@id='lord_1_35']"/>
    <xsl:template match="Hero[@id='lord_1_25']"/>
    <xsl:template match="Hero[@id='lord_1_23']"/>
    <xsl:template match="Hero[@id='lord_1_11']"/>
    <xsl:template match="Hero[@id='lord_1_111']"/>
    <xsl:template match="Hero[@id='lord_1_12']"/>
    <xsl:template match="Hero[@id='lord_1_36']"/>
    <xsl:template match="Hero[@id='lord_1_26']"/>
    <xsl:template match="Hero[@id='lord_1_40']"/>
    <xsl:template match="Hero[@id='lord_1_40_1']"/>
    <xsl:template match="Hero[@id='lord_1_46']"/>
    <xsl:template match="Hero[@id='lord_1_46_1']"/>
    <xsl:template match="Hero[@id='lord_1_45']"/>
    <xsl:template match="Hero[@id='lord_1_45_1']"/>
    <xsl:template match="Hero[@id='lord_1_45_2']"/>
    <xsl:template match="Hero[@id='lord_1_45_3']"/>
    <xsl:template match="Hero[@id='lord_1_57']"/>
    <xsl:template match="Hero[@id='lord_1_57_1']"/>
    <xsl:template match="Hero[@id='lord_1_57_2']"/>
    <xsl:template match="Hero[@id='lord_1_52']"/>
    <xsl:template match="Hero[@id='lord_1_52_1']"/>
    <xsl:template match="Hero[@id='lord_1_52_2']"/>
    <xsl:template match="Hero[@id='lord_1_62']"/>
    <xsl:template match="Hero[@id='lord_1_62_1']"/>
    <xsl:template match="Hero[@id='lord_1_53']"/>
    <xsl:template match="Hero[@id='lord_1_73']"/>
    <xsl:template match="Hero[@id='lord_1_73_1']"/>
    <xsl:template match="Hero[@id='lord_1_71']"/>
    <xsl:template match="Hero[@id='lord_WE8_c']"/>
    <xsl:template match="Hero[@id='lord_WE8_u']"/>
    <xsl:template match="Hero[@id='lord_WE9_l']"/>
    <xsl:template match="Hero[@id='lord_WE9_u']"/>
    <xsl:template match="Hero[@id='lord_WE9_u2']"/>

    Leave: lord_1_7 and lord_1_8
    ```

### lords.xslt

Now let's remove lords by keeping the ruler and the spouse.

??? example "Aserai:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='dead_lord_3_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_7']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_13']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_13_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_13_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_8']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_11']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_3_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_51']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_20']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_20_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_20_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_14']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_14_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_15_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_15_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_16_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_17_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_17_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_21']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_21_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_18']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_18_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_18_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_18_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_18_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_19']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_19_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_19_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_19_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_23']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_23_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_22']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_22_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_22_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_22_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_3_22_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_A9_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_A9_s']"/>
    <xsl:template match="NPCCharacter[@id='lord_A9_c']"/>
    <xsl:template match="NPCCharacter[@id='lord_A9_u']"/>

    Leave: lord_3_1 and lord_3_2
    ```

Examples for other Kingdoms:

??? example "Vlandia:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='lord_4_7']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_13']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_14']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_141']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_3_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_8']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_11']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_6_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_121']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_16_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_18']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_181']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_19']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_25']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_25_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_21']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_20']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_20_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_22']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_22_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_23']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_23_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_23_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_23_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_24']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_24_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_24_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_24_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_24_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_26']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_26_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_27']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_27_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_V9_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_28']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_28_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_4_28_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_V11_l']"/>    
    <xsl:template match="NPCCharacter[@id='lord_V11_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_V11_c1']"/>
    <xsl:template match="NPCCharacter[@id='lord_V11_c2']"/>

    Leave: lord_4_1 and lord_4_2
    ```

??? example "Khuzait:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='dead_lord_6_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_7']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_101']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_13']"/>
    <xsl:template match="NPCCharacter[@id='dead_lord_6_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_8']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_81']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_11']"/>
    <xsl:template match="NPCCharacter[@id='dead_lord_6_4']"/>    
    <xsl:template match="NPCCharacter[@id='lord_6_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_51']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_15_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_15_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_16_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_16_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_24']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_17_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_17_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_21']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_21_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_18']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_18_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_18_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_22']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_22_1']"/>    
    <xsl:template match="NPCCharacter[@id='lord_6_19']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_19_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_19_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_23']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_20']"/>
    <xsl:template match="NPCCharacter[@id='lord_6_20_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_K8_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_K9_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_K9_s']"/>
    <xsl:template match="NPCCharacter[@id='lord_K9_c1']"/>
    <xsl:template match="NPCCharacter[@id='lord_K9_c2']"/> 

    Leave: lord_6_1 and lord_6_2
    ```

??? example "Battania:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='lord_5_3_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_19']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_14']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_14_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_22']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_16_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_17_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_B8_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_1_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_3_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_5_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_7']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_8']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_91']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_11']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_13']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_13_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_131']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_15_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_15_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_15_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_16_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_18']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_18_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_20']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_21']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_21_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_5_21_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_B8_s']"/>
    <xsl:template match="NPCCharacter[@id='lord_B8_c']"/>

    Leave: lord_5_1 (no spouse)
    ```

??? example "Sturgia:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='dead_lord_2_1']"/>
    <xsl:template match="NPCCharacter[@id='dead_lord_2_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_13_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_13_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_13_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_13_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_7']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_7_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_13']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_4_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_8']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_11']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_111']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_14']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_121']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_14_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_14_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_14_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_16_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_21']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_21_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_17_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_22']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_22_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_24']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_24_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_18']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_18_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_23']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_23_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_15_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_15_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_15_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_15_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_19']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_19_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_20']"/>
    <xsl:template match="NPCCharacter[@id='lord_2_20_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_S8_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_S9_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_S9_m']"/>
    <xsl:template match="NPCCharacter[@id='lord_S9_c']"/>
    <xsl:template match="NPCCharacter[@id='lord_S9_u']"/>

    Leave: lord_2_1 and lord_2_2
    ```


??? example "Empire South:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='lord_1_27']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_27_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_27_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_27_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_37']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_47']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_47_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_47_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_47_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_155']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_28']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_38']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_48']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_48_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_48_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_48_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_177']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_29']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_18']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_39']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_30']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_30_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_49']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_49_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_49_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_30_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_30_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_56_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_63_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_63_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_63']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_63_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_74']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_74_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_54']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_54_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_68']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_68_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_69_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_69']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_69_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_55']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_55_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_72']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_72_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_SE8_c']"/>
    <xsl:template match="NPCCharacter[@id='lord_SE9_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_SE9_s']"/>
    <xsl:template match="NPCCharacter[@id='lord_SE9_c1']"/>
    <xsl:template match="NPCCharacter[@id='lord_SE9_c2']"/>

    Leave: lord_1_14 (no spouse)
    ```

??? example "Empire North:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='lord_1_20']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_41']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_411']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_31']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_21']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_22']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_42']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_32']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_422']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_4']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_5']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_7']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_8']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_6']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_33']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_43']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_11']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_13']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_14']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_64']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_17']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_50']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_66']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_15']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_1_16']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_51']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_67']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_58']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_70']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE7_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE8_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE8_s']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE8_c1']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE8_c2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_56']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_56_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE9_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE9_s']"/>
    <xsl:template match="NPCCharacter[@id='lord_NE9_d']"/>

    Leave: lord_1_1 and lord_1_2
    ```

??? example "Empire West:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='lord_1_75']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_34']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_24']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_44']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_9']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_10']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_35']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_25']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_23']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_11']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_111']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_12']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_36']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_26']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_40']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_40_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_46']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_46_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_45']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_45_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_45_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_45_3']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_57']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_57_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_57_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_52']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_52_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_52_2']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_62']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_62_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_53']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_73']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_73_1']"/>
    <xsl:template match="NPCCharacter[@id='lord_1_71']"/>
    <xsl:template match="NPCCharacter[@id='lord_WE8_c']"/>
    <xsl:template match="NPCCharacter[@id='lord_WE8_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_WE9_l']"/>
    <xsl:template match="NPCCharacter[@id='lord_WE9_u']"/>
    <xsl:template match="NPCCharacter[@id='lord_WE9_u2']"/>

    Leave: lord_1_7 and lord_1_8
    ```

### clans.xslt

Now remove all clans, except the ruling clan.

??? example "Aserai:"
    ``` xml
    <xsl:template match="Faction[@id='clan_aserai_2']"/>
    <xsl:template match="Faction[@id='clan_aserai_3']"/>
    <xsl:template match="Faction[@id='clan_aserai_4']"/>
    <xsl:template match="Faction[@id='clan_aserai_5']"/>
    <xsl:template match="Faction[@id='clan_aserai_6']"/>
    <xsl:template match="Faction[@id='clan_aserai_7']"/>
    <xsl:template match="Faction[@id='clan_aserai_8']"/>
    <xsl:template match="Faction[@id='clan_aserai_9']"/>

    Leave: clan_aserai_1
    ```

Examples for other Kingdoms:

??? example "Vlandia:"
    ``` xml
    <xsl:template match="Faction[@id='clan_vlandia_2']"/>
    <xsl:template match="Faction[@id='clan_vlandia_3']"/>
    <xsl:template match="Faction[@id='clan_vlandia_4']"/>
    <xsl:template match="Faction[@id='clan_vlandia_5']"/>
    <xsl:template match="Faction[@id='clan_vlandia_6']"/>
    <xsl:template match="Faction[@id='clan_vlandia_7']"/>
    <xsl:template match="Faction[@id='clan_vlandia_8']"/>
    <xsl:template match="Faction[@id='clan_vlandia_9']"/>
    <xsl:template match="Faction[@id='clan_vlandia_10']"/>
    <xsl:template match="Faction[@id='clan_vlandia_11']"/>

    Leave: clan_vlandia_1
    ```

??? example "Khuzait:"
    ``` xml
    <xsl:template match="Faction[@id='clan_khuzait_2']"/>
    <xsl:template match="Faction[@id='clan_khuzait_3']"/>
    <xsl:template match="Faction[@id='clan_khuzait_4']"/>
    <xsl:template match="Faction[@id='clan_khuzait_5']"/>
    <xsl:template match="Faction[@id='clan_khuzait_6']"/>
    <xsl:template match="Faction[@id='clan_khuzait_7']"/>
    <xsl:template match="Faction[@id='clan_khuzait_8']"/>
    <xsl:template match="Faction[@id='clan_khuzait_9']"/>

    Leave: clan_khuzait_1
    ```

??? example "Battania:"
    ``` xml
    <xsl:template match="Faction[@id='clan_battania_2']"/>
    <xsl:template match="Faction[@id='clan_battania_3']"/>
    <xsl:template match="Faction[@id='clan_battania_4']"/>
    <xsl:template match="Faction[@id='clan_battania_5']"/>
    <xsl:template match="Faction[@id='clan_battania_6']"/>
    <xsl:template match="Faction[@id='clan_battania_7']"/>
    <xsl:template match="Faction[@id='clan_battania_8']"/>
    <xsl:template match="Faction[@id='clan_battania_9']"/>

    Leave: clan_battania_1
    ```

??? example "Sturgia:"
    ``` xml
    <xsl:template match="Faction[@id='clan_sturgia_2']"/>
    <xsl:template match="Faction[@id='clan_sturgia_3']"/>
    <xsl:template match="Faction[@id='clan_sturgia_4']"/>
    <xsl:template match="Faction[@id='clan_sturgia_5']"/>
    <xsl:template match="Faction[@id='clan_sturgia_6']"/>
    <xsl:template match="Faction[@id='clan_sturgia_7']"/>
    <xsl:template match="Faction[@id='clan_sturgia_8']"/>
    <xsl:template match="Faction[@id='clan_sturgia_9']"/>

    Leave: clan_sturgia_1
    ```


??? example "Empire South:"
    ``` xml
    <xsl:template match="Faction[@id='clan_empire_south_2']"/>
    <xsl:template match="Faction[@id='clan_empire_south_3']"/>
    <xsl:template match="Faction[@id='clan_empire_south_4']"/>
    <xsl:template match="Faction[@id='clan_empire_south_5']"/>
    <xsl:template match="Faction[@id='clan_empire_south_6']"/>
    <xsl:template match="Faction[@id='clan_empire_south_7']"/>
    <xsl:template match="Faction[@id='clan_empire_south_8']"/>
    <xsl:template match="Faction[@id='clan_empire_south_9']"/>

    Leave: clan_empire_south_1
    ```

??? example "Empire North:"
    ``` xml
    <xsl:template match="Faction[@id='clan_empire_north_2']"/>
    <xsl:template match="Faction[@id='clan_empire_north_3']"/>
    <xsl:template match="Faction[@id='clan_empire_north_4']"/>
    <xsl:template match="Faction[@id='clan_empire_north_5']"/>
    <xsl:template match="Faction[@id='clan_empire_north_6']"/>
    <xsl:template match="Faction[@id='clan_empire_north_7']"/>
    <xsl:template match="Faction[@id='clan_empire_north_8']"/>
    <xsl:template match="Faction[@id='clan_empire_north_9']"/>

    Leave: clan_empire_north_1
    ```

??? example "Empire West:"
    ``` xml
    <xsl:template match="Faction[@id='clan_empire_west_2']"/>
    <xsl:template match="Faction[@id='clan_empire_west_3']"/>
    <xsl:template match="Faction[@id='clan_empire_west_4']"/>
    <xsl:template match="Faction[@id='clan_empire_west_5']"/>
    <xsl:template match="Faction[@id='clan_empire_west_6']"/>
    <xsl:template match="Faction[@id='clan_empire_west_7']"/>
    <xsl:template match="Faction[@id='clan_empire_west_8']"/>
    <xsl:template match="Faction[@id='clan_empire_west_9']"/>

    Leave: clan_empire_west_1
    ```

### Test->Backup

Run a NEW game, it should not crash. All heroes should be gone, clans gone. Only ruling clan should remain with the ruler and the spouse. Also the game will generate some new noname lords to fill the ranks, ignore them. Check in Encyclopedia.

If all works:

!!! danger "Make a full backup of your /Modules/YOUR_MOD folder under a new name"


## Remove the Kingdom

Assign ruling clan_1 to another Kingdom AND comment out the original Kingdom

Example with Aserai:

Assigning clan to a new Kingdom in clans.xslt (super_faction=...(make sure you have such a kingdom)):
``` xml
<xsl:template match="Faction[@id='clan_aserai_1']">
    <Faction id="clan_aserai_1"
        name="Warmia"
        tier="5"
        owner="Hero.lord_3_1"
        culture="Culture.baltic"
        super_faction="Kingdom.prussia"
        is_noble="true"
        banner_key="11.2.140.1836.1836.768.788.1.0.-30.449.35.127.402.517.764.762.1.0.180" />
</xsl:template>
```

Removing the old Kingdom in kingdoms.xslt:

``` xml
<xsl:template match="Kingdom[@id='aserai']"/>
```

### Test->Backup

Run a NEW game, it should not crash. Old clan with his ruler/spouse should belong to a new Kingdom. Check in Encyclopedia.

If all works:

!!! danger "Make a full backup of your /Modules/YOUR_MOD folder under a new name"


## Cleanup

Change the old ruler's and his spouse's details in lords.xslt.

!!! danger "KEEP THE ORIGINAL IDs!"

??? example "Example with Aserai ruler:"
    ``` xml
    <xsl:template match="NPCCharacter[@id='lord_3_1']">
        <NPCCharacter id="lord_3_1" name="Glappo" voice="curt" age="33" is_female="false" is_hero="true" culture="Culture.baltic" occupation="Lord" default_group="Cavalry">
            <face>
                <!-- custom face -->
                <BodyProperties version="4" age="33.33" weight="0.2114" build="0.8191"  key="00132C0C40A0101C77648A8630040F47B0F076B4BA828F036F0D041073F12409897A962405F870B9C368D90006956C6900000000000000250000000040AC7040"  />
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
                <EquipmentSet id="baltic_lord_equipmentset_template"/>
                <EquipmentSet id="baltic_lord_equipmentset_template_civilian" civilian="true"/>
            </Equipments>
        </NPCCharacter>
    </xsl:template>
    ```

### Test->Backup

Run a NEW game, it should not crash. Old lords should have new names. Check in Encyclopedia.

If all works:

!!! danger "Make a full backup of your /Modules/YOUR_MOD folder under a new name"


## Final words

I wrote this from my records, not testing it along the way. There will be mistakes and ommisions. Let me know if so - I will fix the guide. (Litauen)
