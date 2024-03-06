# Animations

* [Animations - Official Doc](https://moddocs.bannerlord.com/asset-management/asset-types/animations/)

Make agent perform some animation:

``` cs
ActionIndexCache myAction = ActionIndexCache.Create("nameOfTheAction");
targetAgent.SetActionChannel(0, myAction, true);
```