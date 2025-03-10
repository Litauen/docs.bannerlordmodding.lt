# Custom character backgrounds

!!! info "Guide by TheMadProphet"

`CharacterCreationState` is a [Game State](/modding/game_states/) which controls what options should be displayed during character creation.

We can create and inject our own `CharacterCreationState`, which can either extend or completely replace the existing options.

By default, game uses `SandboxCharacterCreationContent`, which is created inside `SandBoxGameManager`:
```cs
private void LaunchSandboxCharacterCreation()
{
    CharacterCreationState characterCreationState = Game.Current.GameStateManager.CreateState<CharacterCreationState>( new object[] { new SandboxCharacterCreationContent() } );  
    Game.Current.GameStateManager.CleanAndPushState(characterCreationState, 0);
}
```

Using [harmony](/modding/harmony/), we can modify this method and insert our own state:
```cs
[HarmonyPatch]
public class CharacterCreationPatch
{
    [HarmonyPrefix]
    [HarmonyPatch(typeof(SandBoxGameManager), "LaunchSandboxCharacterCreation")]
    public static bool LaunchCustomCharacterCreation()
    {
        var characterCreationState = Game.Current.GameStateManager.CreateState<CharacterCreationState>( new CustomCharacterCreationContent() );
        Game.Current.GameStateManager.CleanAndPushState(characterCreationState, 0);

        return false;
    }
}
```

Finally, we will need to implement `CustomCharacterCreationContent`.

It can be implemented either from scratch (by inheriting `CharacterCreationContentBase`), or it could extend the existing options (by inheriting `SandboxCharacterCreationContent`).

## Example: Adding new options
This is a starting file for adding new options.
It includes optional `AddOption` method, which makes adding new options a bit easier and more readable.
```cs
public class CustomCharacterCreationContent : SandboxCharacterCreationContent
{
    protected override void OnInitialized(CharacterCreation characterCreation)
    {
        base.OnInitialized(characterCreation);  // Keep old options

        // And add a new option in childhood stage
        AddOption(
            characterCreation,
            CharacterCreationMenu.Childhood,  // At which stage should this option be shown
            new TextObject("your curiosity for the arcane."),  // Option title
            new MBList<SkillObject> { DefaultSkills.Charm },  // Which skills to increase
            DefaultCharacterAttributes.Social,  // Which attribute to increase
            FocusToAdd,  // Default 1
            SkillLevelToAdd,  // Default 10
            AttributeLevelToAdd,  // Default 1
            new TextObject("Lorem ipsum")  // Description text
        );
    }

    // Helper method to add new options
    private static void AddOption(
        CharacterCreation characterCreation,
        CharacterCreationMenu menu,
        TextObject text,
        MBList<SkillObject> skills,
        CharacterAttribute attribute,
        int focus,
        int skillLevel,
        int attributeLevel,
        TextObject description
    )
    {
        var characterCreationMenu = characterCreation.CharacterCreationMenus[(int)menu];
        foreach (var cultureCategory in characterCreationMenu.CharacterCreationCategories)
        {
            cultureCategory.AddCategoryOption(
                text,
                skills,
                attribute,
                focus,
                skillLevel,
                attributeLevel,
                null,  // Option condition: a delegate that returns true if the option should be shown
                null, // On select: a delegate that is called when the option is selected
                Empty, // On apply: a delegate that is called when the option is applied (If this is null skills won't increase, so we need to pass at least an empty method)
                description
            );
        }
    }

    private static void Empty(CharacterCreation characterCreation) { }

    private enum CharacterCreationMenu
    {
        Parents,
        Childhood,
        Education,
        Youth,
        Adult,
        Age
    }
}
```

For more advanced options and customization (e.g. changing character, pose, etc.), you can have a look at the existing options in `SandboxCharacterCreationContent`.
