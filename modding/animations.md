# Animations

Make agent perform some animation:

``` cs
ActionIndexCache myAction = ActionIndexCache.Create("nameOfTheAction");
targetAgent.SetActionChannel(0, myAction, true);
```