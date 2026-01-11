# scene.xscene

`\SceneObj\Main_map\scene.xscene` Main Map's scene XML file (edit with any text/XML editor)

## 1.3 DLC related changes

### town

`<game_entity name="town_EW1" old_prefab_name="" mobility="1">`

needs:

```xml
<script name="BlockadePositionScript">
    <variables>
        <variable name="MaximumNumberOfShips" value="8"/>
        <variable name="NumberOfArcs" value="4"/>
        <variable name="DistanceBetweenShips" value="1.545"/>
        <variable name="DistanceRandomizationOnArcs" value="0.100"/>
        <variable name="DistanceRandomizationBetweenArcs" value="0.100"/>
        <variable name="Angle" value="1.921"/>
        <variable name="MissionShipId" value="dromon_ship_nested"/>
        <variable name="ShipScaleFactor" value="0.045"/>
        <variable name="IsVisualizationEnabled" value="true"/>
        <variable name="IsRandomizationEnabled" value="false"/>
        <variable name="IsShipVisualizationEnabled" value="false"/>
    </variables>
</script>

...

<game_entity name="town_port" old_prefab_name="__empty_object" mobility="1">
    <tags>
        <tag name="main_map_city_port"/>
    </tags>
    <transform position="-5.141, 2.057, -1.571" rotation_euler="0.000, 0.000, -1.848"/>
</game_entity>
<game_entity name="town_blockade_end" old_prefab_name="__empty_object" mobility="1">
    <tags>
        <tag name="Blockade_Arc_End"/>
    </tags>
    <transform position="-4.047, 6.966, -1.696" rotation_euler="-0.602, -0.067, -1.723"/>
</game_entity>
<game_entity name="town_blockade_start" old_prefab_name="__empty_object" mobility="1">
    <tags>
        <tag name="Blockade_Arc_Start"/>
    </tags>
    <transform position="-6.496, -2.380, -1.393" rotation_euler="-0.602, -0.042, -1.723"/>
</game_entity>
```

??? info "How it looks in the Editor (v1.3.13)"
    ![](/pics/2601101412.png)


### Fishing Village

Each fishing village should have 'drop_point' entity for fishing parties to spawn.

```xml
<game_entity name="drop_point" old_prefab_name="" mobility="1">
    <tags>
        <tag name="main_map_village_dropoff"/>
    </tags>
    <transform position="1, 1, 0" rotation_euler="0, 0, 0"/>
</game_entity>
```

!!! note "Adjust the coordinates so the entity would be on the water near the village."

!!! abstract "Related code: FishingPartyCampaignBehavior - CanHaveFishingParties"

### Pirate Spawn Points

```xml
<game_entity name="southern_pirates" old_prefab_name="" mobility="1">
    <transform position="488.148, 215.637, 2.580" rotation_euler="0.000, 0.000, 0.000"/>
    <scripts>
        <script name="PirateSpawnPoint">
            <variables>
                <variable name="ClanStringId" value="southern_pirates"/>
                <variable name="ToggleDebugRadius" value="false"/>
                <variable name="Radius" value="30.000"/>
            </variables>
        </script>
    </scripts>
</game_entity>

<game_entity name="northern_pirates" old_prefab_name="" mobility="1">
    <transform position="554.548, 529.875, 2.580" rotation_euler="0.000, 0.000, 0.000"/>
    <scripts>
        <script name="PirateSpawnPoint">
            <variables>
                <variable name="ClanStringId" value="northern_pirates"/>
                <variable name="ToggleDebugRadius" value="false"/>
                <variable name="Radius" value="30.000"/>
            </variables>
        </script>
    </scripts>
</game_entity>
```

!!! note "In the DLC's scene.xscene southern_pirates have 26 spawn points, northern_pirates - 22"

!!! warning "If you add Pirate Spawn Points xml directly to the scene.xscene over text/XML editor, open file in the Editor and Save scene in the Editor, all &lt;scripts> part will be gone"
    Tested with Editor v1.3.13