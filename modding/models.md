# Models

* [Patching game models with Harmony](/modding/harmony/#patching-game-models)

!!! quote "Windwhistle:"

Models are used for specific calculations/outcomes related to the game.

Most (if not all) DEFAULT models can be found within

    TaleWorlds.CampaignSystem.GameComponents

These are good for looking at more in-depth for knowledge. These are what the game is using by default.

Most (if not all) model INTERFACES can be found within

    TaleWorlds.CampaignSystem.ComponentInterfaces

It is best to inherit from an interface model if using the decorator pattern, as far as I can tell, to ensure that you are making full use of the previous model's return values for all interface methods.

    public class NewAgeModel : AgeModel

There's quite a lot more models to choose from. The class names speak for themselves thankfully.

![](/pics/2402182140.png)




## Best Implementation Method

[Spinozart](https://discord.com/channels/411286129317249035/677511186295685150/1208527753603981322): About DefaultModel: If another mod is using the same Model than your mod, it will override everything if it is loaded after yours. 

[carbon](https://discord.com/channels/411286129317249035/677511186295685150/1208571938658582570): You can do [this](https://gist.github.com/carbon198/e198ffcf1021c7ad1226ffb850ebccb5). That way at least you can tell people to load your mod last and it won’t be an issue.

[[BUTR] Aragas](https://discord.com/channels/411286129317249035/677511186295685150/1050115613621755995): You're forgetting the [decorator pattern](https://www.dofactory.com/net/decorator-design-pattern):

``` cs
public class BModel : Model
{
    private Model _previousModel;

    public override void DoSomething()
    {
        _previousModel.DoSomething();
    }
}

// Somewhere
var existingModel = gameStarter.GetGameModel(); //AModel
gameStarter.AddModel(new BModel(existingModel));
```

[Windwhistle](https://discord.com/channels/411286129317249035/677511186295685150/1208679299729858581): So the key to it is gameStarter.GetGameModel() it seems which means that you don't need to depend on anything.

!!! example "[Spinozart](https://discord.com/channels/411286129317249035/677511186295685150/1208732219380736051): It would be nice to precise that the GetGameModel() doesn't exist. We have to create this Array method to look for the required Model through IGameStarter.Models. carbon shared the GitHub of Diplomacy, and we can find how it is all settled in the [SubModule.cs](https://github.com/DiplomacyTeam/Bannerlord.Diplomacy/blob/main/src/Bannerlord.Diplomacy/SubModule.cs):"

    ``` cs
    protected override void OnGameStart(Game game, IGameStarter gameStarter)
    {
        base.OnGameStart(game, gameStarter);

        var currentKingdomDecisionPermissionModel = GetGameModel<KingdomDecisionPermissionModel>(gameStarter);
        if (currentKingdomDecisionPermissionModel is null)
            Log.LogWarning("No default KingdomDecisionPermissionModel found!");     // custom Log implementatino here. Use your own

        gameStarter.AddModel(new DiplomacyKingdomDecisionPermissionModel(currentKingdomDecisionPermissionModel));
    }

    private T? GetGameModel<T>(IGameStarter gameStarterObject) where T : GameModel
    {
        var models = gameStarterObject.Models.ToArray();

        for (int index = models.Length - 1; index >= 0; --index)
        {
            if (models[index] is T gameModel1)
                return gameModel1;
        }
        return default;
    }
    ```


!!! example "[carbon](https://discord.com/channels/411286129317249035/677511186295685150/1208733697793331240): if you write out a stub of the model class, inheriting from abstract base class and with a constructor that takes previousModel, visual studio will create the rest of the class for you:"
    ![](/pics/model_implement_demo.gif)


### Tradeoffs

!!! quote "Eagle:"
    Everything has trade-offs. Using the decorator pattern improves mod compatibility but it also generates lots of boilerplate code. It can harm code readability.
!!! quote "Windwhistle:"
    Not much imo, and the alternative is your mod being incompatible with anything else using the same game model. <br>
    I would say the boilerplate code is very worth it.<br>
    Unless, there is another method that can be used without requiring the other mod to be a dependency<br>
    The only scenario I could see where it's not worth it to use this method would be for the big overhaul mods that override everything anyway.<br>
    For smaller mods, I would say this method is a complete must.
!!! quote "carbon:"
    There's definitely a tradeoff. The major pro is you can be compatible with overhaul mods that use most of the game models, but don't necessarily call their base methods. This means patching the default model methods will do nothing.
!!! quote "Eagle:"
    At any given time, a bannerlord update could change the model API. This could break a given mod relying on them. Mods relying on Harmony postfixes might not and also be mod compatible.<br>
    It's far-fetched. I'm just playing the devil's advocate to point that all solutions have trade-offs. We should not mindlessly go for a given solution because someone said it's better. They should analyse their problem and use a solution which fits their needs.<br>
    But I agree with all your points.


## Validation

!!! quote "carbon:"

I made a little validator you can put in OnGameInitializationFinished() or really anywhere after the models are added to check whether another model overrode yours. It checks to see if they are using the decorator pattern, and doesn't give an error if they are. It's not perfect, but should work in most [cases](https://gist.github.com/carbon198/d15c8299c389ac6c0f9fd241e7216652):

``` cs
private void ValidateGameModel(GameModel model)
{
  if (model.GetType().Assembly == GetType().Assembly) { return; }
  if (!model.GetType().BaseType.IsAbstract)
  {
    TextObject error = new("Game Model Error: Please move " + GetType().Assembly.GetName().Name + " below " + model.GetType().Assembly.GetName().Name + " in your load order to ensure mod compatibility");
    InformationManager.DisplayMessage(new InformationMessage(error.ToString(),Colors.Red));
  }
}
```

Usage:

``` cs
public override void OnGameInitializationFinished(Game game)
{
    base.OnGameInitializationFinished(game);

    ValidateGameModel(Campaign.Current.Models.VolunteerModel);
}
```


* Check for other mods [example](/modules/check_for_other_mods/)
