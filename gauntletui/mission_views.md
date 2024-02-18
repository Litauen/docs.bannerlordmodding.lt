# MissionViews

- [Missions](/modding/missions/)

## Changes in 1.2.9

!!! danger "MissionView.OnMissionScreenInitialize() does not have access to the MissionScreen anymore. (Crash on base.MissionScreen.AddLayer(layer);)"


!!! quote "[Dejan](https://discord.com/channels/411286129317249035/677511186295685150/1204033796513595432) commenting on 1.2.9 change:"

    MissionViews are registered at OnMissionAfterStarting() and MissionScreen is initialized at OnMissionLoadingFinished().
    If there are unregistered views when initializing missionscreen they could fail.
    Views should only be added using MissionScreen.AddMissionView or via the ViewMethod methods.
    And we did not destroy MissionView, we have just changed how missionscreen initialization works.
    We now use "MissionScreen.AddMissionView(missionView)" instead of "missionView.MissionScreen = missionScreen".
    This was done to prevent leaks and inconsistent behaviors.



Create your screen in the OnBehaviorInitialize(), it is possible to access base.MissionScreen there.

Make sure you add your MissionView in OnMissionBehaviorInitialize (not in OnBeforeMissionBehaviorInitialize).

Add [DefaultView] tag to your CustomMissionView:

``` cs
[DefaultView]
public class CustomMissionView : MissionView
```
