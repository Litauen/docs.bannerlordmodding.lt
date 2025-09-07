# Animations

* [Animations - Official Doc](https://moddocs.bannerlord.com/asset-management/asset-types/animations/)
* [Bannerlord Animation Documentation](https://docs.google.com/document/d/1fTZ80cahM0A4bgmTa9ermnBZiQY2APJ_sEXz22oKRjo/edit?tab=t.0)


## Export/Import

!!! quote "Kemo III"

    Get TpacTools and export the animation to FBX, and then import to blender.<br>
    You'll find info on https://docs.bannerlordmodding.lt/ about the correct import/export settings in Blender.<br>
    It's going to need some clean up before you can edit it well because it has key frames on every frame.<br>
    And make sure when you're importing the fbx back to Bannerlord to only import skeletal animations and not the skeleton.<br>
    Once you import the animation back to BL, you'll find you have a new skeletal animation resource. Right click anywhere and create-override whichever animation clip you want to override.


## Create a new clip in the Editor

!!! quote "Kemo III:"
    Make a new animation clip.<br>
    Right click in the resource browser and create an override (there's a bug if you try creating one that's not an override)<br>
    You can then rename it if you don't want to override an existing clip.

## SetActionChannel

Activates animation on the agent

``` cs
Agent.SetActionChannel(1, ActionIndexCache.act_none, false, (ulong)agent.GetCurrentActionPriority(1), 0f, 1f, 2f, 0.4f, 0f, false, -0.2f, 0, true);

public bool SetActionChannel(int channelNo, ActionIndexCache actionIndexCache, bool ignorePriority = false, ulong additionalFlags = 0UL, float blendWithNextActionFactor = 0f, float actionSpeed = 1f, float blendInPeriod = -0.2f, float blendOutPeriodToNoAnim = 0.4f, float startProgress = 0f, bool useLinearSmoothing = false, float blendOutPeriod = -0.2f, int actionShift = 0, bool forceFaceMorphRestart = true)


ActionIndexCache myAction = ActionIndexCache.Create("nameOfTheAction");
targetAgent.SetActionChannel(0, myAction, true);
```

## channelNo

for agent.SetActionChannel(0, _actionIndexCache);, what's the difference between channel 0 and channel 1?

channel 0 is for movement, channel 1 is for combat anims (like swings and stuff)


!!! quote "hunharibo: afaik 0 is full body, 1 is upper body only (leg anims are still driven by movement)"


## blendInPeriod

float `blendInPeriod` - sets how fast animation should blend-in from the previous animation. Sometimes the best looking results to go to the .act_none state are achieved with 1f or 2f values

default -0.2f is too fast ant looks unnatural


## Example

??? example "Cheer example"
    ``` cs
    private readonly ActionIndexCache[] _CheerActions = new ActionIndexCache[]
    {
        ActionIndexCache.Create("act_cheering_low_01"),
        ActionIndexCache.Create("act_cheering_low_02"),
        ActionIndexCache.Create("act_cheering_low_03"),
        ActionIndexCache.Create("act_cheering_low_04"),
        ActionIndexCache.Create("act_cheering_low_05"),
        ActionIndexCache.Create("act_cheering_low_06"),
        ActionIndexCache.Create("act_cheering_low_07"),
        ActionIndexCache.Create("act_cheering_low_08"),
        ActionIndexCache.Create("act_cheering_low_09"),
        ActionIndexCache.Create("act_cheering_low_10"),
        ActionIndexCache.Create("act_cheer_1"),
        ActionIndexCache.Create("act_cheer_2"),
        ActionIndexCache.Create("act_cheer_3"),
        ActionIndexCache.Create("act_cheer_4"),
        ActionIndexCache.Create("act_cheering_high_01"),
        ActionIndexCache.Create("act_cheering_high_02"),
        ActionIndexCache.Create("act_cheering_high_03"),
        ActionIndexCache.Create("act_cheering_high_04"),
        ActionIndexCache.Create("act_cheering_high_05"),
        ActionIndexCache.Create("act_cheering_high_06"),
        ActionIndexCache.Create("act_cheering_high_07"),
        ActionIndexCache.Create("act_cheering_high_08")
    };

    agent.SetActionChannel(1, _CheerActions[MBRandom.RandomInt(_CheerActions.Length)], false, 0UL, 0f, 1f, 1f, 0.4f, 0f, false, -0.2f, 0, true);
    agent.MakeVoice(SkinVoiceManager.VoiceType.Victory, SkinVoiceManager.CombatVoiceNetworkPredictionType.NoPrediction);

    // stop cheering (after some time)
    affectorAgent.SetActionChannel(1, ActionIndexCache.act_none, false, (ulong)agent.GetCurrentActionPriority(1), 0f, 1f, 2f, 0.4f, 0f, false, -0.2f, 0, true);
    ```


## Stop looping

When agent plays a looping animation set with SetActionChannel to stop it set it to act_none action.

## Revert to default animation

```cs
agent.SetActionChannel(1, ActionIndexCache.act_none, ignorePriority: true);
```

## Pause the animation

```cs
agent.SetCurrentActionSpeed(0, 0f);
```

## Facial animations

!!! quote "Artem:"

Facial animations won't work properly if the agent is using a game object - each idle that an agent performs inside missions have a designated game object tied to them, like standing guard or tavern wench serving and there only the first frame of the facial animation will get used for some reason (weird behavior). You'd need to cancel the animation and call it again for facial animations to work - in the case of my bard here is the code that worked:
```cs
if (courtJester.GetCurrentAction(0).Name.Contains("act_artemfeast_playing_lyre_bard") && courtJester.IsUsingGameObject)
{
    courtJester.StopUsingGameObject(true); //makes agent stop using the object
    courtJester.SetActionChannel(0, ActionIndexCache.act_none, true); //sets the action to none
    courtJester.SetActionChannel(0, ActionIndexCache.Create("act_artemfeast_playing_lyre_bard"), true); //sets the new animation
    courtJester.SetAgentFacialAnimation(Agent.FacialAnimChannel.High, "talking_engaged", true); //adds facial animation via loop (facial animations already work without this, but I want it to loop that's why I put it here)
}
```

## Workflow

!!! quote "Artem:"

export with 24 fps and import into blender with proper orientation x and - y

Do my editing, export work proper values and import into bannerlord