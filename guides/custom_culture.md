# Custom Culture

!!! warning "WARNING: This guide is for v1.2.12"


![](/pics/c2soKfU.png)


On new game start character culture can be selected if it's marked in the culture XML as is_main_culture="true":

``` xml
<Culture
    id="baltic"
    name="Baltic"
    is_main_culture="true"
```

## Culture's Selection Cards

![](/pics/dAbzEt7.png)


The culture's name that is visible under the picture is defined in the module_strings.xml :

``` xml
<string id="str_culture_rich_name.baltic" text="Baltic" />
```

![](/pics/nnjvduV.png)


Image dimensions for the culture's picture: 366 x 668

I leave some transparent space at the bottom to not cover the culture's name.

Also round the corners, so the highlight would not look weird.

![](/pics/cPK9nuC.png)

These images should be named the same as culture ID:

``` xml
<Culture
    id="baltic"
```

Image name: baltic.png

Sprites should be [imported](/gauntletui/sprites/#import) from:

    \MODULE_NAME\GUI\SpriteParts\ui_charactercreation\CharacterCreation\Culture\

That means that we are overwriting the native sprite category: ui_charactercreation

and vanilla culture images will be gone. If we want them back - export them with [TpacTool/BannerEdge](/resources/tools/) and reimport them together with your new culture's sprites with the correct names: battania, aserai, etc.

The game uses these sprites for your new cultures automatically - nothing to be done here anymore.


### Remove Native Cultures

To get rid of native cultures from this selection:

![](/pics/2402281143.png)

It is necessary to remove them completely from the game (quite hard with all the relations/units/etc) or make: is_main_culture=false in their XMLs using [XSLT](/modding/xml/#xslt):

``` xml
<xsl:template match="Culture[@id='vlandia']/@is_main_culture">
    <xsl:attribute name="is_main_culture">false</xsl:attribute>
</xsl:template>

<xsl:template match="Culture[@id='empire']/@is_main_culture">
    <xsl:attribute name="is_main_culture">false</xsl:attribute>
</xsl:template>

<xsl:template match="Culture[@id='aserai']/@is_main_culture">
    <xsl:attribute name="is_main_culture">false</xsl:attribute>
</xsl:template>

<xsl:template match="Culture[@id='sturgia']/@is_main_culture">
    <xsl:attribute name="is_main_culture">false</xsl:attribute>
</xsl:template>

<xsl:template match="Culture[@id='battania']/@is_main_culture">
    <xsl:attribute name="is_main_culture">false</xsl:attribute>
</xsl:template>

<xsl:template match="Culture[@id='khuzait']/@is_main_culture">
    <xsl:attribute name="is_main_culture">false</xsl:attribute>
</xsl:template>
```

Also it's necessary to disable one method in the code using Harmony:

``` cs
// No sorting/expecting of native cultures
[HarmonyPatch(typeof(CharacterCreationCultureStageVM))]
[HarmonyPatch("SortCultureList")]
public class CharacterCreationCultureStageVM_SortCultureList_Patch
{
    public static bool Prefix()
    {
        return false;
    }
}
```

### Custom Music

When selecting different culture's card. Details [here](/modding/sound/#custom-music-in-culture-selection-menu)

<br>
## Culture's Description

![](/pics/SYqApNY.png)


The moving images are constructed out of 4 sprites. Each of them is stacked on top of another as visible in this example:

![](/pics/2zkCAS4.png)

4 at the bottom, does not move. Then sprite #3 - moves very slightly, #2 - moves more and #1 on the top, moves the most.

These sprites move horizontally at different speeds trying to create an illusion.

We will need 4 separate images (with some transparency to not cover each other completely) with the names: CULTURE_1, CULTURE_2, CULTURE_3, CULTURE_4, where CULTURE is your culture's ID, for example: baltic_1, baltic_2, baltic_3, baltic_4

Dimensions for a picture: 952 x 876

Leave some transparent space at the top/bottom for a better look. The full image start at the top of the screen and goes behind the text - not very nice.


These sprites should be imported from the same folder: 

    \MODULE_NAME\GUI\SpriteParts\ui_charactercreation\CharacterCreation\Culture\

Then we must create custom brushes to allow the game to use our newly created sprites.

For that we need to copy

    \Mount & Blade II Bannerlord\Modules\Native\GUI\Brushes\CharacterCreation.xml
to

    \OUR_MOD\GUI\Brushes\CharacterCreation.xml

and add our sprites to brushes: "Culture.Banner.Layer.4", "Culture.Banner.Layer.3", "Culture.Banner.Layer.2", "Culture.Banner.Layer.1".

Example for "Culture.Banner.Layer.3":

``` xml
  <Brush Name="Culture.Banner.Layer.3">
    <Layers>
      <BrushLayer Name="Default"  />
    </Layers>
    <Styles>
      <Style Name="baltic">
        <BrushLayer Name="Default" Sprite="CharacterCreation\Culture\baltic_3" />
      </Style>
```

And that will be all to show our custom cultures pictures.

Demo:

<center>
    <iframe width="640" height="360" src="https://www.youtube.com/embed/LRH9zWHL7sY" frameborder="0" allowfullscreen></iframe>
</center>



## You were born into a family of...

With the custom culture, this window is empty by default:

![](/pics/NyqeknN.png)

because menu selections are hardcoded for vanilla cultures only.

??? info "Click here to see menus for vanilla cultures"
    ![](/pics/61myENz.png)
    ![](/pics/PuOXRlv.png)
    ![](/pics/feLgRDw.png)
    ![](/pics/Gdu5u5J.png)
    ![](/pics/co4ShqN.png)
    ![](/pics/hiHlbOV.png)

To make this menu work for your custom culture we need a simple Harmony patch (1.2.12):

``` cs
[HarmonyPatch(typeof(SandboxCharacterCreationContent))]
[HarmonyPatch("VlandianParentsOnCondition")]
public class SandboxCharacterCreationContentVlandianParentsOnConditionPatch
{
    public static void Postfix(ref bool __result, SandboxCharacterCreationContent __instance)
    {
        if (__instance.GetSelectedCulture().StringId == "baltic") __result = true;
    }
}
```

This patch tells to use the Vlandia menu for custom baltic culture.

Select the best vanilla culture for your custom culture and apply this patch with the necessary changes to have this menu working.



## Coronation scene

Without changes coronation scene could look like this with the new culture:

![](/pics/2409180854.png)

The default troop id for the coronation guards is 'fighter_sturgia' - so if you removed it - guards will have no clothes.

Use this patch to fix this, change cultures/troopIDs in the example to yours:

```cs
[HarmonyPatch(typeof(CampaignSceneNotificationHelper), "GetBodyguardOfCulture")]
public class CampaignSceneNotificationHelper_GetBodyguardOfCulture_Patch
{
    private static bool Prefix(ref SceneNotificationData.SceneNotificationCharacter __result, CultureObject culture)
    {
        string stringId = culture.StringId;
        string troopId = "balt_14";
        if (stringId == "baltic")
        {
            troopId = "balt_21";
        }
        else if (stringId == "crusader")
        {
            troopId = "crusader_26";
        }
        else if (stringId == "rus")
        {
            troopId = "rus_20";
        }
        __result = new SceneNotificationData.SceneNotificationCharacter(MBObjectManager.Instance.GetObject<CharacterObject>(troopId), null, default(BodyProperties), false, uint.MaxValue, uint.MaxValue, false);           
        return false;
    }
}
```

The title in the string should be set with such text variables in module_strings.xml for each of your new culture:

```xml
<string id="str_liege_title.baltic" text="{=str_liege_title.baltic}Grand Duke" />
<string id="str_liege_title_female.baltic" text="{=str_liege_title_female.baltic}Grand Duchess" />
```

Fixed scene:

![](/pics/2409180859.png)


## Dialog Text/Voices

Make your culture's NPCs talk in different dialects.

Change your culture's accordingly.

```cs
[HarmonyPatch(typeof(ConversationManager), "FindMatchingTextOrNull")]
public static class ConversationManager_FindMatchingTextOrNull_Patch
{
    static void Prefix(CharacterObject character, out CultureObject __state)
    {
        // Save the original culture
        __state = character.Culture;

        // Temporarily assign the fake culture
        string fakeCulture = "";
        if (__state.StringId == "baltic" || __state.StringId == "latvian" || __state.StringId == "estonian")
        {
            fakeCulture = "battania";
        }
        else if (__state.StringId == "crusader")
        {
            fakeCulture = "vlandia";
        }
        else if (__state.StringId == "danish")
        {
            fakeCulture = "empire";
        }
        else if (__state.StringId == "rus" || __state.StringId == "polish")
        {
            fakeCulture = "sturgia";
        }
        // aserai / khuzait

        if (fakeCulture != "") character.Culture = Game.Current.ObjectManager.GetObjectTypeList<CultureObject>().FirstOrDefault(c => c.StringId == fakeCulture);
    }

    // Restore the original culture
    static void Postfix(CharacterObject character, CultureObject __state)
    {
        character.Culture = __state;
    }
}
```
!!! warning "Some users reported that this patch does not always work properly. Use it at your own risk."

## Other changes

* [Menu backgrounds](/guides/custom_menu_background/)
* [Starting positions](/guides/custom_start_positions/)
