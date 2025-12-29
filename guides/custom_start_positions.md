# Custom Start Positions

!!! abstract "For 1.3+"
    Set in culture.xml file:
    ```xml
    start_point_position_x="213.01"
    start_point_position_y="471.19"
    ```

!!! warning "For 1.2.12:"

Example Harmony patch. Set your own cultures/coordinates.

Get player's coordinates with:

    campaign.print_main_party_position


```cs
// Custom start positions

[HarmonyPatch(typeof(SandboxCharacterCreationContent))]
[HarmonyPatch("OnCharacterCreationFinalized")]
public class SandboxCharacterCreationContent_CustomStartingPositions_Patch
{
    public static bool Prefix()
    {

        CultureObject culture = CharacterObject.PlayerCharacter.Culture;

        if (culture.StringId == "crusader")
        {
            MobileParty.MainParty.Position2D = new Vec2(239.3924f, 585.5247f); // crusader near Arensburg
        }
        else if (culture.StringId == "rus")
        {
            MobileParty.MainParty.Position2D = new Vec2(457.39f, 478.97f); // rus near Vitebsk
        }
        else if (culture.StringId == "danish")
        {
            MobileParty.MainParty.Position2D = new Vec2(352.05f, 644.98f); // danish near Narva
        }
        else
        {
            MobileParty.MainParty.Position2D = new Vec2(302.4088f, 401.8863f); // baltic near Merkine
        }

        MapState mapState;
        bool flag2 = (mapState = GameStateManager.Current.ActiveState as MapState) != null;
        if (flag2)
        {
            mapState.Handler.ResetCamera(true, true);
            mapState.Handler.TeleportCameraToMainParty();
        }

        return false;
    }
}
```
