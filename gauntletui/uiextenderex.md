# UIExtenderEx

* [Homepage](https://uiextenderex.butr.link/index.html)


## Activation

??? info "Should be included into project's .scproj file"
    ``` cs
    <ItemGroup>
        <PackageReference Include="Nullable" Version="1.3.1" PrivateAssets="all" IncludeAssets="runtime; build; native; contentfiles; analyzers; buildtransitive" />
        <PackageReference Include="IsExternalInit" Version="1.0.3" PrivateAssets="all" IncludeAssets="runtime; build; native; contentfiles; analyzers; buildtransitive" />
        <PackageReference Include="Bannerlord.BuildResources" Version="1.0.1.85" PrivateAssets="all" IncludeAssets="runtime; build; native; contentfiles; analyzers; buildtransitive" />
        <PackageReference Include="Lib.Harmony" Version="2.2.2" IncludeAssets="compile" />
        <PackageReference Include="Harmony.Extensions" Version="3.1.0.67" PrivateAssets="all" IncludeAssets="runtime; build; native; contentfiles; analyzers; buildtransitive" />
        <PackageReference Include="BUTR.Harmony.Analyzer" Version="1.0.1.44" PrivateAssets="all" IncludeAssets="runtime; build; native; contentfiles; analyzers; buildtransitive" />
        <PackageReference Include="Bannerlord.UIExtenderEx" Version="2.8.0" IncludeAssets="compile" />
    </ItemGroup>
    ```

    Should be visible in the Dependencies:

    ![](https://imgur.com/PV7e8IX.png)

    ![](https://imgur.com/4ULLtZa.png)

??? info "Register in the OnSubModuleLoad()"

    ``` cs
      public class CustomSubModule : MBSubModuleBase
      {
          protected override void OnSubModuleLoad()
          {
              base.OnSubModuleLoad();

              _extender = new UIExtender("ModuleName");
              _extender.Register(typeof(CustomSubModule).Assembly);
              _extender.Enable();
          }
        }
    ```


## Notes

* Needs game restart on every change in the code... (At least editing main map)
* Does not throw any exceptions no matter what nonsense is written here: [PrefabExtension("BLABLA", "descendant::ListPanel[@Id='EventsListPanel']")]
* Crashes, when there is an error in XML: document.LoadXml("<Widget IsVisible=\"@IsInRange\"> <Children> <Widget Id=\"TrackerFrame\"...
* When UIExtenderEx is loaded, it is possible to copy game's /GUI/Prefabs/*.xml files into own mod's /GUI/Prefabs folder and change them to see quick result of the change
