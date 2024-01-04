# Items

## Tutorials

* [Creating Custom Items - Part 1](https://www.youtube.com/watch?v=U-qAa4cmf28&list=PLxhni8XI_dRDjRRDsCzEBZg4eUInzkzQT&index=2){target=_blank}
* [Creating Custom Items - Part 2](https://www.youtube.com/watch?v=IKyJD9dzTEI&list=PLxhni8XI_dRDjRRDsCzEBZg4eUInzkzQT&index=3){target=_blank}



## ItemObject

``` cs
ItemObject grain = MBObjectManager.Instance.GetObject<ItemObject>("grain");
```

## ItemRoster

Class to describe all items the Party/Town/Hero has.

??? example "Count all food items in the ItemRoster"

    ``` cs
    int itemCount = 0;
    for (int i = 0; i < itemRoster.Count; i++)
    {
        ItemObject item = itemRoster.GetItemAtIndex(i);
        if (item.IsFood)
        {
            itemCount += itemRoster.GetItemNumber(item);
        }
    }
    ```

How many grain player party has:

``` cs
PartyBase.MainParty.ItemRoster.GetItemNumber(MBObjectManager.Instance.GetObject<ItemObject>("grain"));
```

How many grain settlement has:

``` cs
Settlement.ItemRoster.GetItemNumber(MBObjectManager.Instance.GetObject<ItemObject>("grain"));
```

## Item Price

Returns weird price, not the same as in the Trade screen.

town.GetItemPrice or village.GetItemPrice:

``` cs
int GetItemPrice(ItemObject item, MobileParty tradingParty = null, bool isSelling = false)
int GetItemPrice(EquipmentElement itemRosterElement, MobileParty tradingParty = null, bool isSelling = false)
```

<br>

This returns the same price as in the Trade screen.

town.MarketData.GetPrice or village.MarketData.GetPrice


``` cs
int GetPrice(ItemObject item, MobileParty tradingParty, bool isSelling, PartyBase merchantParty)

int itemPrice = village.MarketData.GetPrice(item, MobileParty.MainParty, false, village.Settlement.Party);
```

<br>
item.value - returns default item's value (from XML?)


## InventoryManager

Screen(s) to trade/exchange items

``` cs
InventoryManager.OpenScreenAsReceiveItems(itemRoster, new TextObject("Top Left Name"), new InventoryManager.DoneLogicExtrasDelegate(OnInventoryScreenDone));
```

??? example "More functions"

    ``` cs
    public static void OpenScreenAsInventoryOfSubParty(MobileParty rightParty, MobileParty leftParty, DoneLogicExtrasDelegate doneLogicExtrasDelegate) // not used in-game, crash on exit
    public static void OpenScreenAsInventoryForCraftedItemDecomposition(MobileParty party, CharacterObject character, DoneLogicExtrasDelegate doneLogicExtrasDelegate)
    public static void OpenScreenAsInventory(DoneLogicExtrasDelegate doneLogicExtrasDelegate = null
    public static void OpenScreenAsReceiveItems(ItemRoster items, TextObject leftRosterName, DoneLogicExtrasDelegate doneLogicDelegate = null)
    public static void OpenScreenAsTrade(ItemRoster leftRoster, SettlementComponent settlementComponent, InventoryCategoryType merchantItemType = InventoryCategoryType.None, DoneLogicExtrasDelegate doneLogicExtrasDelegate = null)
    public static void OpenScreenAsInventoryOf(MobileParty party, CharacterObject character)
    public static void OpenScreenAsInventoryOf(PartyBase rightParty, PartyBase leftParty)
    public static void OpenScreenAsLoot(Dictionary<PartyBase, ItemRoster> itemRostersToLoot)
    public static void OpenScreenAsStash(ItemRoster stash)
    public static void OpenTradeWithCaravanOrAlleyParty(MobileParty caravan, InventoryCategoryType merchantItemType = InventoryCategoryType.None)
    ```



## Item XML folder

    \Modules\SandBoxCore\ModuleData\items

    Basic trade goods described in this file:
    \Modules\SandBoxCore\ModuleData\items\horses_and_others.xml

For mods:

    \MODFolder\ModuleData

## Get Items in-game

Trade goods: Items.AllTradeGoods<br>
Other: Items.All.Where(item => (whatever condition you want))

``` cs
Items.All.Where(item => item.Type == ItemObject.ItemTypeEnum.Goods && !item.StringId.Contains("book")))

// get ordered by Name
var orderedItems = Items.All
  .Where(item => item.Type == ItemObject.ItemTypeEnum.Goods && !item.StringId.Contains("book"))
  .OrderBy(item => item.Name.ToString(), StringComparer.Ordinal);

foreach (ItemObject item in orderedItems) { // do something with the items }
```

## XML

??? tip "PROBLEM: Items are not loaded from XML"
    Check your engine logs for item read errors/warning. As I said before a new behavior that I experienced with 1.2++ versions of the game that if you have a faulty item, all the item definitions coming after it will not load either, not just the one that is bad. So maybe there has always been one that was bad, you just didnt notice it among the hundreds of items. ([hunharibo](https://discord.com/channels/411286129317249035/677511186295685150/1187684885457010690))


### Properties

* 'mesh' - 3D mesh
* 'using_tableau' - value is used to determine whether or not the item can be displayed in the game's 3D model viewer, also known as the tableau
* 'is_merchandise' - is used to determine whether an item is a merchandise or not. Used in the game code to determine if Item can be lost, on deciding rewards and something else
* 'appearance' - used in calculating the item value (CalculateValue): return (int)(num2 * num * (1f + 0.2f * (item.Appearance - 1f)) + 100f * MathF.Max(0f, item.Appearance - 1f));
* 'value' - price
* 'weight'
* 'culture' (optional) - Defined culture of the current item. If an item is set for a culture, the item will only be sold on the markets of the same culture. Can't have multiple values of the 'culture' attribute, so that the product is available for sale in towns of only two cultures. You can set it to neutral culture, but then it will show up everywhere. 
* 'difficulty' - (need confirmation) is used to indicate the level of difficulty in crafting an item
* 'item_category' - (need confirmation) influences price range based on the market. Example - if item_category="jewelry" and item value="1000", in-game item can cost from ~1000 at start and drop to 150 after few weeks. Because jewelry prices drop in the region. If this item_category is changed to let's say 'book', then in-game same item costs ~1000 again.
* 'subtype' - is used to provide additional information about the type of item being defined. The subtype property is often used in conjunction with the item_category property to further refine the classification of an item

### ItemObject

Required skill/value:

    ItemObject.RelevantSkill, ItemObject.Difficulty


### Type

??? "ItemTypeEnum"
    * Invalid
    * Horse
    * OneHandedWeapon
    * TwoHandedWeapon
    * Polearm
    * Arrows
    * Bolts
    * Shield
    * Bow
    * Crossbow
    * Thrown
    * Goods
    * HeadArmor
    * BodyArmor
    * LegArmor
    * HandArmor
    * Pistol
    * Musket
    * Bullets
    * Animal
    * Book
    * ChestArmor
    * Cape
    * HorseHarness
    * Banner

Type="Goods" sets ItemObject.IsTradeGood == true and ItemObject.ItemType == "Goods"

??? warning "Caravan Ambush mission"

    Gives 3 random "Goods" type items as a reward. So if your item type is "Goods", they will be given at some point.


Q. How to make certain items not appear in the lootpool and also in the marketplace? <br>
A. Set is_merchandise="false" and assign to the culture that does not have markets, e.g. Looters/Bandits (Culture.looter)

It appears that items that are not for sale (is_merchandise="false") may appear as prizes in tournaments.
But it probably depends on the cost of the item - the higher it is, the greater the likelihood.



<br>
## How to find ItemID

Let's say we want to find ItemID for this item:

![](https://imgur.com/hkirJAc.png)


Use [ripgrep](/resources/tools/) in the game's folder:

    rg -C 5 "Thinhide Coif"

And you will see (among a lot of other stuff):

``` xml
Modules\SandBoxCore\ModuleData\items\head_armors.xml
869-  </Item>
870-  <!-- #region Battania Armors -->
871-  <!-- #region Battania Head Armors -->
872-  <!-- #region Battania Light Head Armors -->
873-  <Item id="thinhide_coif"
874:        name="{=Q50hfNjW}Thinhide Coif"
```

This is what you need:

``` xml
<Item id="thinhide_coif"
```

Then you can use it in your mod. Example:

``` xml
<Equipments>
    <EquipmentRoster civilian="true">
        <equipment slot="Head"
            id="Item.thinhide_coif" />
```


## Helmet Hair Coverage

??? abstract "Helmet Hair Coverage"

    ![](https://imgur.com/Iq2hAG0.png)
