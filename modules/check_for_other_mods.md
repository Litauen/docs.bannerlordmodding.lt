# Check for other mods

In SubModule.cs:

``` cs
public override void OnGameInitializationFinished(Game game)    // executes after all mods are loaded
{
    base.OnGameInitializationFinished(game);
    try
    {
        string[] modulesNames = Utilities.GetModulesNames();
        for (int i = 0; i < modulesNames.Length; i++)
        {
            if (modulesNames[i] == "BannerKings")
            {
                // do something about it
            }
        }
    }
    catch (Exception ex)
    {
        // do something about it
    }
}
```
