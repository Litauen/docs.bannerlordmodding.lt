# Game States


## Game.State

``` cs
public enum State
{
    Running,
    Destroying,
    Destroyed
}

if (Game.Current?.CurrentState == Game.State.Running) ...
```

Initial menu - no game state. As soon as loaded - Game.State.Running. Exit menu, other menus, time is not going - still "Running"




## GameStateManager.ActiveState

### IsMenuState

```
if (Game.Current?.GameStateManager.ActiveState.IsMenuState) ...
```

False when settlement menu is opened.

Active when some window is opened:

    BannerBuilder
    BannerEditor
    Barber
    CharacterDeveloper
    Clan
    Crafting
    Education
    GameOver
    Inventory
    Kingdom
    Party
    Quest
    Tutorial

### IsMission

```
if (Game.Current?.GameStateManager.ActiveState.IsMission) ...
```

When Mission is active

