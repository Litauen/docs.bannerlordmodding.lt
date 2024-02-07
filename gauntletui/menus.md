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
        return true;
    },
    delegate (MenuCallbackArgs args)
    {
        InformationManager.DisplayMessage(new InformationMessage("My menu works!"));
    }, false, 1, false);
}

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

## Background Image

``` cs
args.MenuContext.SetBackgroundMeshName("wait_raiding_village");
```


Defined in Modules\Native\GUI\NativeSpriteData.xml

Can use custom 445x805 sprite there.


- [Settlements Menu Image](/modding/settlements/#wait_mesh)
- [Encounters](/modding/cultures/#xml) (encounter_background_mesh in cultures.xml)



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

??? info "AddWaitGameMenu"


    ``` cs
    Basic example:

    campaignGameStarter.AddWaitGameMenu("wait_menu_name", "top text", delegate (MenuCallbackArgs args)
    {
        args.MenuContext.GameMenu.SetTargetedWaitingTimeAndInitialProgress(10f, 0f);
    }, delegate (MenuCallbackArgs args)
    {
        return true;
    }, delegate (MenuCallbackArgs args)
    {
        GameMenu.ExitToLast();
    }, delegate (MenuCallbackArgs args, CampaignTime dt)
    {
        args.MenuContext.GameMenu.SetProgressOfWaitingInMenu((float)this._startTimeOfWaiting.ElapsedHoursUntilNow / 10);
    }, GameMenu.MenuAndOptionType.WaitMenuShowOnlyProgressOption, GameOverlays.MenuOverlayType.None, 0f, GameMenu.MenuFlags.None, null);

    campaignGameStarter.AddGameMenuOption("wait_menu_name", "leave", "Leave", delegate (MenuCallbackArgs args)
    {
        args.optionLeaveType = GameMenuOption.LeaveType.Leave;
        return true;
    }, delegate (MenuCallbackArgs args)
    {
        GameMenu.ExitToLast();
    }, false, -1, false);
    ```

args.MenuContext.GameMenu.SetTargetedWaitingTimeAndInitialProgress(10, 0f); - set's how long to wait

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

