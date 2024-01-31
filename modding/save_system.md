# Save System

## Tutorials

* [TW Documentation](https://docs.bannerlordmodding.com/_csharp-api/savesystem/){target=_blank}
* [Community Documentation](https://docs.bannerlordmodding.com/_csharp-api/savesystem/){target=_blank}
* [ButterLib SaveSystem](https://butr.github.io/Bannerlord.ButterLib/articles/SaveSystem/Overview.html){target=_blank}
* [Lesser Scholar Youtube tutorial - Saving your Mod Data](https://www.youtube.com/watch?v=RteqVUpEN8E&list=PLzebdAxJeltRwfJ8jzsNolgHkRvLjoCRC&index=15){target=_blank}
* [Danqna and Gispry Youtube tutorial - Saving and Game Ticks](https://www.youtube.com/watch?v=zTLSyhXsepc&list=PL02GptyKz6G0oElRE_27efBWQIE1zsAgd&index=6){target=_blank}
* [Cheyros Saving Data Tutorial](https://forums.taleworlds.com/index.php?threads/saving-data.415921/){target=_blank}


## Observations

* Custom trade items in the inventory are saved/loaded without additional coding
* Created custom wanderer is saved/loaded without additional coding
* When mod is removed, custom trade items in the inventory are converted into Trash Item(s)
* When mod is removed, custom Wanderer transforms into usual Wanderer which can be hired as a Companion
* Adding new SaveableField into the Saveable Class works ok the with the old save (no crash)
* Removing old SaveableField from the Saveable Class works ok with the old save (no crash)
* Saving with the custom class, changing class to the struct, and loading - works. Vice-versa also, when structure of class and struct are the same. Weird...


## Example

??? example "Example with 2 custom classes, one inside another in a list"

    ``` cs

    [SaveableRootClass(1)]
    public class TradeData
    {
        [SaveableField(1)]
        public Hero Hero;

        [SaveableField(2)]
        public bool Active;

        [SaveableField(3)]
        public int Balance;

        [SaveableField(4)]
        public ItemRoster Stash;

        [SaveableField(5)]
        public List<TradeItemData> TradeItemsDataList;

        public TradeData(Hero hero)
        {
            this.Hero = hero;
            this.Active = false;
            this.Balance = 0;
            this.Stash = new ItemRoster();

            this.TradeItemsDataList = new List<TradeItemData>();
        }
    }


    [SaveableRootClass(2)]
    public class TradeItemData
    {
        [SaveableField(1)]
        public ItemObject item;

        [SaveableField(2)]
        public float minPrice;

        [SaveableField(3)]
        public float maxPrice;

        public TradeItemData(ItemObject item)
        {
            this.item = item;
            this.minPrice = 0;
            this.maxPrice = -1;
        }
    }


    public class CustomSaveDefiner : SaveableTypeDefiner
    {
        public CustomSaveDefiner() : base(YOUR_MOD_UNIQUE_ID)  { }

        protected override void DefineClassTypes()
        {
            base.AddClassDefinition(typeof(TradeData), 1);
            base.AddClassDefinition(typeof(TradeItemData), 2);
        }

        protected override void DefineContainerDefinitions()
        {
            base.ConstructContainerDefinition(typeof(Dictionary<Hero, TradeData>));
            base.ConstructContainerDefinition(typeof(List<TradeItemData>));
        }
    }
    ```


## CampaignBehaviour.SyncData

Defines what we are saving/loading:

``` cs
dataStore.SyncData<int>("_bookInProgress", ref this._bookInProgress);
dataStore.SyncData<float[]>("_bookProgress", ref this._bookProgress);
dataStore.SyncData<Dictionary<Hero, TradeData>>("_tradeData", ref MyBehaviour.TradeAgentsData);
```

## Save data leaking to a new game

!!! danger "Not properly handled custom data from the save can leak to a new game (Campaign or Sandbox)!"

    How to reproduce:

    1. Have a mod that saves custom data and shows it on start, every hour, etc - should have an easy way to check
    2. Load a save file with such data
    3. Exit this to the main menu
    4. Start Campaign or Sandbox game
    5. Run it
    6. Observe how data from your save file is still present in the new game

    ??? example "More info to reproduce"
        Artem:  I think I'm uncovering something here, I tested the idea of global saving on different mods that save data, specifically https://www.nexusmods.com/mountandblade2bannerlord/mods/4948 and https://www.nexusmods.com/mountandblade2bannerlord/mods/5249
        In healthy relationships rewrite I won a tournament in one save file which enables you to dedicate this tournament victory to an npc. I saved the file, started a new campaign and the option to dedicate the tournament is still there even though it shouldn't be. Same with small talk, I complimented someone, gained relation, saved, started a new game, complimented the same person again and got no relation increase?

    Fix:

    Init/clear your custom data in OnNewGameCreated:

    ``` cs
    public override void RegisterEvents()
    {
        CampaignEvents.OnNewGameCreatedEvent.AddNonSerializedListener(this, new Action<CampaignGameStarter>(this.OnNewGameCreated));
    }

    private void OnNewGameCreated(CampaignGameStarter starter)
    {
        YourCustomData.Clear(); // example with the Dictionary type
    }
    ```

## Saving Custom Class

??? example "Needs CustomSaveDefiner, like this:"

    ``` cs
    public class CustomSaveDefiner : SaveableTypeDefiner
    {
        public CustomSaveDefiner() : base(YOUR_MOD_UNIQUE_ID)  { }

        protected override void DefineClassTypes()
        {
            base.AddClassDefinition(typeof(TradeData), 1);
        }

        protected override void DefineContainerDefinitions()
        {
            base.ConstructContainerDefinition(typeof(Dictionary<Hero, TradeData>));
        }
    }
    ```


## Unique SaveDefiner() : base(YOUR_MOD_UNIQUE_ID)

From [Cheyros Saving Data Tutorial](https://forums.taleworlds.com/index.php?threads/saving-data.415921/){target=_blank}

Use CRC32 hash based on your mod's name to help avoid conflicts with the other mods.

 Put your mod's name into [this tool](https://www.fileformat.info/tool/hash.htm){target=_blank} and then scroll down to get the first 6 digits of your mod's CRC32 hash.

Example: AIValuesLife = c5e07920 -> just use first 6 digits e07920<br>

This will ensure you use a base id unique to your mod... people need to do this or else you will have problems saving with many mods installed.

!!! danger "base(YOUR_MOD_UNIQUE_ID) is used to define custom classes"

    That means that each custom class has unique ID, which is generated from YOUR_MOD_UNIQUE_ID + the 2nd parameter from the base.AddClassDefinition

    ``` cs
    protected void AddClassDefinition(Type type, int saveId, IObjectResolver resolver = null)
    {
        TypeDefinition classDefinition = new TypeDefinition(type, _saveBaseId + saveId, resolver);
        _definitionContext.AddClassDefinition(classDefinition);
    }
    ```

    Example:

    ``` cs
    public CustomSaveDefiner() : base(100)  { }

    protected override void DefineClassTypes()
    {
        base.AddClassDefinition(typeof(TradeData), 1);
    }
    ```

    Class TradeData will have ID 101 (100+1) and it's unique per game!

    If another mod has base(99) and another custom class with base.AddClassDefinition(typeof(someOtherClass), 2); - the game will crash on load.

    Because 99+2 = 101 and this ID is already taken.

    ??? danger "Example log on such crash:"

        In the C:/ProgramData/Mount and Blade II Bannerlord/logs/rgl_log_errors_....txt

        ![](/pics/AMFkSht.png)


## Saving Custom Struct

``` cs
base.AddStructDefinition(typeof(YOUR_STRUCT), 1);
