# Menus

## Tutorials/Resources

* [Lesser Scholar - Ep 14: Quick Way to edit Menus](https://www.youtube.com/watch?v=WIsGqcGOeZQ){target=_blank}
* [Main Game Menu editing](https://www.nexusmods.com/mountandblade2bannerlord/mods/5233){target=_blank}

## Basic example

Adds new menu option into Village menu

``` cs
public override void RegisterEvents()
{
    CampaignEvents.OnSessionLaunchedEvent.AddNonSerializedListener(this, new Action<CampaignGameStarter>(this.OnSessionLaunched));
}

private void OnSessionLaunched(CampaignGameStarter starter)
{
    AddGameMenus(starter);
}

private void AddGameMenus(CampaignGameStarter starter)
{
    starter.AddGameMenuOption("village", "test_menu", "My test menu",
    (MenuCallbackArgs args) => {
        args.optionLeaveType = GameMenuOption.LeaveType.HostileAction;
        args.Tooltip = new TextObject("{GOLD_ICON} Tooltip! {GOLD_ICON}", null);
        return true;
    },
    delegate (MenuCallbackArgs args)
    {
        InformationManager.DisplayMessage(new InformationMessage("My menu works!"));
    }, false, 1, false);
}

```

## Another example

Town menu:

```cs
private void AddHolyPlaceMenus(CampaignGameStarter starter)
{
    starter.AddGameMenuOption("town", "holy_place", "Go to the {HOLY_PLACE}",
    (MenuCallbackArgs args) => {
        args.optionLeaveType = GameMenuOption.LeaveType.Submenu;
        // set dynamic holy place name - church, mosque, etc
        MBTextManager.SetTextVariable("HOLY_PLACE", "holy place", false);
        return true;
    },
    delegate (MenuCallbackArgs args)
    {
        GameMenu.SwitchToMenu("holy_place");
    }, false, 1, false);

    starter.AddGameMenu("holy_place", "{MENU_TEXT}",
    (MenuCallbackArgs args) => {

        // set proper background picture
        // args.MenuContext.SetBackgroundMeshName("some_bg_picture");

        // set dynamic menu text
        MBTextManager.SetTextVariable("MENU_TEXT", "You are in the holy place...", false);
    }, TaleWorlds.CampaignSystem.Overlay.GameOverlays.MenuOverlayType.SettlementWithBoth);

    starter.AddGameMenuOption("holy_place", "holy_place_enter", "Enter inside", (MenuCallbackArgs args) =>
    {
        args.optionLeaveType = GameMenuOption.LeaveType.Mission;
        return true;
    }, (MenuCallbackArgs args) =>
    {
        InformationManager.DisplayMessage(new InformationMessage("Closed!"));
    }, true);

    starter.AddGameMenuOption("holy_place", "leave", "Leave", (MenuCallbackArgs args) =>
    {
        args.optionLeaveType = GameMenuOption.LeaveType.Leave;
        return true;
    }, (MenuCallbackArgs args) => { GameMenu.SwitchToMenu("town"); }, true);
}
```

## Open menu

```cs
GameMenu.SwitchToMenu("holy_place");
```


## Menus in-game

- "town"
    - "town_keep"
    - "town_arena"
    - "town_backstreet" Tavern District
- "castle"
- "village"


## AddGameMenu

``` cs
public void AddGameMenu(
    string menuId,
    string menuText,
    OnInitDelegate initDelegate,
    GameOverlays.MenuOverlayType overlay = GameOverlays.MenuOverlayType.None,
    GameMenu.MenuFlags menuFlags = GameMenu.MenuFlags.None,
    object relatedObject = null)
```

### Background Image

``` cs
args.MenuContext.SetBackgroundMeshName("wait_raiding_village");
```


Defined in Modules\Native\GUI\NativeSpriteData.xml

Can use custom 445x805 sprite there.


- [Settlements Menu Image](/modding/settlements/#wait_mesh)
- [Encounters](/modding/cultures/#xml) (encounter_background_mesh in cultures.xml)


### MenuOverlayType

`TaleWorlds.CampaignSystem.Overlay.GameOverlays.MenuOverlayType.SettlementWithBoth`

Possible types:

```cs
public enum MenuOverlayType
{
    None,
    SettlementWithParties,
    SettlementWithCharacters,
    SettlementWithBoth,
    Encounter
}
```

![](/pics/2411231500.jpg)


## AddGameMenuOption

![](/pics/i2CQmtK.png)


``` cs
public void AddGameMenuOption(
    string menuId,
    string optionId,
    string optionText,
    GameMenuOption.OnConditionDelegate condition,
    GameMenuOption.OnConsequenceDelegate consequence,
    bool isLeave = false,
    int index = -1,
    bool isRepeatable = false)
```


* bool isLeave = false - if True, by pressing Tab goes back to previous menu or to the map
* int index = -1 - order in the menu (some options can be hidden, so will not always show in the exact index you define), -1 - at the very end

### Enabled/Disabled

``` cs
args.IsEnabled = false;
```

### Tooltip

``` cs
args.Tooltip = new TextObject("{=yNMrF2QF}You are wounded", null);
```

### Icon

``` cs
args.optionLeaveType = GameMenuOption.LeaveType.ICON_NAME;
```

??? abstract "ICON_NAMEs 1.1.0+"
    ![](/pics/bWOtObC.png)

??? abstract "ICON_NAMEs 1.0.3"
    ![](/pics/DCeLFMO.png)



## Back to previous menu

    GameMenu.ExitToLast();

## Menu with waiting

Basic example:


``` cs

    private static CampaignTime actionStart = CampaignTime.Now;

    campaignGameStarter.AddWaitGameMenu("wait_menu_name", "top text", delegate (MenuCallbackArgs args)
    {
        // how long to wait
        args.MenuContext.GameMenu.SetTargetedWaitingTimeAndInitialProgress(10f, 0f);

        actionStart = CampaignTime.Now;
    }, delegate (MenuCallbackArgs args) // condition
    {
        return true;
    }, delegate (MenuCallbackArgs args) // consequence
    {
        GameMenu.ExitToLast();
    }, delegate (MenuCallbackArgs args, CampaignTime dt)
    {
        args.MenuContext.GameMenu.SetProgressOfWaitingInMenu((float)actionStart.ElapsedHoursUntilNow / 10);
    }, GameMenu.MenuAndOptionType.WaitMenuShowOnlyProgressOption, GameOverlays.MenuOverlayType.None, 0f, GameMenu.MenuFlags.None, null);

    // if this AddGameMenuOption is missing - progress bar will not be visible
    campaignGameStarter.AddGameMenuOption("wait_menu_name", "leave", "Leave", delegate (MenuCallbackArgs args)
    {
        args.optionLeaveType = GameMenuOption.LeaveType.Leave;
        return true;
    }, delegate (MenuCallbackArgs args)
    {
        GameMenu.ExitToLast();
    },
    true, // (bool isLeave) if true here and there is no GameMenu.ExitToLast() above or similar -> wait menu will hang and game will need a reload
          // if no action is needed - make it false here to avoid hang
    -1, false);
```


!!! warning "args.MenuContext.GameMenu.SetProgressOfWaitingInMenu(100); - stops the waiting (even if previously wait time is not reached yet)"


### ProgressBar

* GameMenu.MenuAndOptionType.WaitMenuShowOnlyProgressOption - only progress bar
* GameMenu.MenuAndOptionType.WaitMenuShowProgressAndHoursOption - writes total wait hours on top of progress bar

!!! warning "Progressbar will not be shown if there are no AddGameMenuOption after the AddWaitGameMenu"


## Sound in menus

``` cs
args.MenuContext.SetPanelSound("event:/ui/panels/settlement_village");
args.MenuContext.SetAmbientSound("event:/map/ambient/node/settlements/2d/village");
```


## Which menu is open?

```cs
if (Campaign.Current.CurrentMenuContext != null)
{
    string currentMenuId = Campaign.Current.CurrentMenuContext.GameMenu.StringId;
}
else
{
    // no menu is open
}
```