# Custom Round Popup (MapNotification)

<center>
![](/pics/5PKXolQ.png)
</center>

## Sprite

[Import 75x75px size round sprite](/gauntletui/sprites){target=_blank} that will be used as a popup image.


In my example it's 'custom_map_notification_sprite'


## BrushLayer/Style

In YOUR_MODULE/GUI/Brushes copy file /SandBox/GUI/Brushes/MapNotification.xml, rename it to CustomMapNotification.xml (or similar, <span style="color:red">do not leave the original name! </span>)


??? danger "About renamed file - still overwrites previous mod with custom brush :("

    If we have such load order of our mod and another mod loaded after our mod and we have such files:

    * /SandBox/GUI/Brushes/MapNotification.xml
    * /OUR_MOD/GUI/Brushes/MapNotification.xml
    * /OTHER_MOD/GUI/Brushes/MapNotification.xml

    then

    /OUR_MOD/GUI/Brushes/MapNotification.xml overwrites /SandBox/GUI/Brushes/MapNotification.xml
    /OTHER_MOD/GUI/Brushes/MapNotification.xml overwrites /OUR_MOD/GUI/Brushes/MapNotification.xml

    if we have like this:

    * /SandBox/GUI/Brushes/MapNotification.xml
    * /OUR_MOD/GUI/Brushes/CustomMapNotification.xml
    * /OTHER_MOD/GUI/Brushes/MapNotification.xml

    then

    /OTHER_MOD/GUI/Brushes/MapNotification.xml overwrites only /SandBox/GUI/Brushes/MapNotification.xml

    and our mod can use our custom Brush from our renamed CustomMapNotification.xml file!

    if we have like this:

    * /SandBox/GUI/Brushes/MapNotification.xml
    * /OUR_MOD/GUI/Brushes/CustomMapNotification.xml
    * /OTHER_MOD/GUI/Brushes/AnotherCustomMapNotification.xml

    then

    AnotherCustomMapNotification.xml overwrites everything :(

    ??? info "Explanation"
        ![](/pics/6KBoYgJ.gif)

        [Link](https://discord.com/channels/411286129317249035/677511186295685150/1112077680276410458){target=_blank}


In this file add

``` xml
<BrushLayer Name="custom_map_notification" Sprite="custom_map_notification_sprite" IsHidden="true" />
```

into

``` xml
<Brushes>
    <Brush Name="Map.Notification.Type.Circle.Image">
        <Layers>
```

and

``` xml
<Style Name="custom_map_notification">
    <BrushLayer Name="custom_map_notification" IsHidden="false" />
</Style>
```

into

``` xml
<Brushes>
    <Brush Name="Map.Notification.Type.Circle.Image">
        <Styles>
```


??? "Should look like this: (... means ommited text)"


    ``` xml
    ...
    <Brush Name="Map.Notification.Type.Circle.Image">
        <Layers>
            ...
            <BrushLayer Name="alley_under_attack" Sprite="SPGeneral\MapNotification\notification_illustration_back_alley" IsHidden="true" />

            <BrushLayer Name="custom_map_notification" Sprite="custom_map_notification_sprite" IsHidden="true" />

        </Layers>
        <Styles>
            ...
            <Style Name="alley_under_attack">
                <BrushLayer Name="alley_under_attack" IsHidden="false" />
            </Style>

            <Style Name="custom_map_notification">
                <BrushLayer Name="custom_map_notification" IsHidden="false" />
            </Style>

        </Styles>
    </Brush>
    ...
    ```



## Code


Add this code:


``` cs
public class CustomMapNotificationVM : MapNotificationItemBaseVM
{
    public CustomMapNotificationVM(CustomMapNotification data) : base(data)
    {
        base.NotificationIdentifier = "custom_map_notification";

        this._onInspect = delegate ()
        {
            // some action on mouse-click on the notification
            SoundEvent.PlaySound2D("event:/ui/notification/quest_start"); // example
        };
    }
}


public class CustomMapNotification : InformationData
{
    public CustomMapNotification(TextObject description) : base(description) { }

    public override TextObject TitleText
    {
        get
        {
            return new TextObject("Can read!"); // not sure where this is used
        }
    }

    public override string SoundEventPath
    {
        get
        {
            return "event:/ui/notification/kingdom_decision";   // play this sound on popup
        }
    }
}


[HarmonyPatch(typeof(MapNotificationVM), "PopulateTypeDictionary")]
internal class PopulateNotificationsPatch
{
    private static void Postfix(MapNotificationVM __instance)
    {
        Dictionary<Type, Type> dic = (Dictionary<Type, Type>)__instance.GetType().GetField("_itemConstructors", BindingFlags.Instance | BindingFlags.NonPublic).GetValue(__instance);
        dic.Add(typeof(CustomMapNotification), typeof(CustomMapNotificationVM));
    }
}


public class CustomSaveDefiner : SaveableTypeDefiner
{
    public CustomSaveDefiner() : base(YOUR_MOD_ID)  { }

    protected override void DefineClassTypes()
    {
        base.AddClassDefinition(typeof(CustomMapNotification), 1);
    }

}

```

??? tip "Make sure you have Harmony activated"

    In your SubModule.cs:

    ``` cs
    protected override void OnSubModuleLoad() {
        base.OnSubModuleLoad();
        Harmony harmony = new Harmony("YOUR_MOD_TEXT_ID");
        harmony.PatchAll();
        ...
    ```


## Execution


Generate the text you want to show in the popup:

Example:

``` cs
TextObject msg = new("{HERO_NAME} learned how to read!");
msg.SetTextVariable("HERO_NAME", hero.Name.ToString());
```

Create popup:

``` cs
Campaign.Current.CampaignInformationManager.NewMapNoticeAdded(new CustomMapNotification(msg));
```


## Credits

Huge thanks to:

* [⚔ Artem ⚔](https://www.youtube.com/@Artem1s){target=_blank} - for the whole code and explanation how to do this with screenshots and examples

* [order_without_power](https://www.nexusmods.com/users/89144138?tab=user+files){target=_blank} - for the help in my fight to modify the Brushes without overwriting them and for the advice to rename the brushes file






