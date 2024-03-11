# Animations

* [Animations - Official Doc](https://moddocs.bannerlord.com/asset-management/asset-types/animations/)

Make agent perform some animation:

``` cs
ActionIndexCache myAction = ActionIndexCache.Create("nameOfTheAction");
targetAgent.SetActionChannel(0, myAction, true);
```

## Channels

for agent.SetActionChannel(0, _actionIndexCache);, what's the difference between channel 0 and channel 1?

channel 0 is for movement, channel 1 is for combat anims (like swings and stuff)


When agent plays a looping animation set with SetActionChannel to stop it set it to act_none action.