# Items

- [ItemObject TaleWorlds.Core.ItemObject Class Reference](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_core_1_1_item_object.html){target=_blank}

## Tutorials

* [Creating Custom Items - Part 1](https://www.youtube.com/watch?v=U-qAa4cmf28&list=PLxhni8XI_dRDjRRDsCzEBZg4eUInzkzQT&index=2){target=_blank}
* [Creating Custom Items - Part 2](https://www.youtube.com/watch?v=IKyJD9dzTEI&list=PLxhni8XI_dRDjRRDsCzEBZg4eUInzkzQT&index=3){target=_blank}


## ItemObject

``` cs
ItemObject grain = MBObjectManager.Instance.GetObject<ItemObject>("grain");
```

??? example "Trade Items:"
    beer<br>
    butter<br>
    cheese<br>
    date_fruit<br>
    fish<br>
    grape<br>
    grain<br>
    meat<br>
    olives<br>

    charcoal<br>
    ironIngot1      (Crude Iron)<br>
    ironIngot2      (Wrought Iron)<br>
    ironIngot3      (Iron)<br>
    ironIngot4      (Steel)<br>
    ironIngot5      (Fine Steel)<br>
    ironIngot6      (Thamaskene Steel)<br>

    clay<br>
    cotton  (Raw Silk)<br>
    flax<br>
    fur<br>
    hardwood<br>
    hides<br>
    jewelry<br>
    leather<br>
    linen<br>
    oil<br>
    pottery<br>
    salt<br>
    silver<br>
    spice<br>
    tools<br>
    velvet<br>
    wine<br>
    wool<br>

    cat<br>
    dog<br>
    chicken<br>
    goose<br>
    hog<br>
    sheep<br>
    cow<br>

    stolen_goods

## ItemRoster

* [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_roster_1_1_item_roster.html)

Class to describe all items the Party/Town/Hero has.


### Properties

``` cs
int     Count [get]
float   TotalWeight [get]
int     TotalFood [get]
int     FoodVariety [get]
int     TotalValue [get]
int     TradeGoodsTotalValue [get]
int     NumberOfPackAnimals [get]
int     NumberOfLivestockAnimals [get]
int     NumberOfMounts [get]
```

### Examples

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

??? example "Check how much of item X the party has example method"
    ``` cs
    public static int GetPartyItemCount(MobileParty party, string itemID)
    {
        if (party == null || party.ItemRoster == null) return 0;
        if (itemID.Length == 0) return 0;

        ItemObject item = MBObjectManager.Instance.GetObject<ItemObject>(itemID);
        if (item == null) return 0;

        return party.ItemRoster.GetItemNumber(item);
    }
    ```


??? example "Check if party has X items example method"
    ``` cs
    public static bool PartyHasItemCount(MobileParty party, string itemID, int count)
    {
        if (party == null || party.ItemRoster == null) return false;
        if (itemID.Length == 0) return false;
        if (count < 0) return false;

        ItemObject item = MBObjectManager.Instance.GetObject<ItemObject>(itemID);
        if (item == null) return false;

        if (party.ItemRoster.GetItemNumber(item) >= count) return true;

        return false;
    }
    ```


Add Item to ItemRoster
``` cs
ItemObject butterItemObject = MBObjectManager.Instance.GetObject<ItemObject>("butter");
if (butterItemObject != null) {
    ItemRosterElement itemRoster = new ItemRosterElement(butterItemObject, 2, null);
    MobileParty.MainParty.ItemRoster.Add(itemRoster);
}
```

??? example "Add X items to the party method example"

    ``` cs
    public static void AddItemsToParty(MobileParty party, string itemID, int count)
    {
        if (party == null || party.ItemRoster == null) return;
        if (itemID.Length == 0) return;
        if (count < 0) return;

        ItemObject item = MBObjectManager.Instance.GetObject<ItemObject>(itemID);
        if (item == null) return;

        party.ItemRoster.Add(new ItemRosterElement(item, count, null));
    }
    ```


Remove
``` cs
ItemRosterElement itemRosterElement = new ItemRosterElement(itemObject, 10, null);
settlement.Stash.Remove(itemRosterElement);     // from settlement's stash
party.ItemRoster.Remove(itemRosterElement);     // from party
```

??? example "Remove X items from the party example method"
    ``` cs
    public static void RemoveItemsFromParty(MobileParty party, string itemID, int count)
    {
        if (party == null || party.ItemRoster == null) return;
        if (itemID.Length == 0) return;
        if (count < 0) return;

        ItemObject item = MBObjectManager.Instance.GetObject<ItemObject>(itemID);
        if (item == null) return;

        party.ItemRoster.Remove(new ItemRosterElement(item, count, null));
    }
    ```


## Sell

```cs
SellItemsAction.Apply(mobileParty.Party, settlement.Party, itemRosterElement, number, settlement);
```

## Price

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

## Tier

    Public enum ItemTiers
    {
        Tier1,
        Tier2,
        Tier3,
        Tier4,
        Tier5,
        Tier6,
        NumTiers
    }

Tier 0 (in GUI) == -1 when comparing with ItemTier.

For a head armor Tier is set automatically based on the &lt;ItemComponent>&lt;Armor head_armor value

By price for other types of armors also:

![](/pics/2402160845.png)

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


### Folder

    \Modules\SandBoxCore\ModuleData\items

    Basic trade goods described in this file:
    \Modules\SandBoxCore\ModuleData\items\horses_and_others.xml

For mods:

    \MODFolder\ModuleData


??? tip "PROBLEM: Items are not loaded from XML"
    Check your engine logs for item read errors/warning. As I said before a new behavior that I experienced with 1.2++ versions of the game that if you have a faulty item, all the item definitions coming after it will not load either, not just the one that is bad. So maybe there has always been one that was bad, you just didnt notice it among the hundreds of items. ([hunharibo](https://discord.com/channels/411286129317249035/677511186295685150/1187684885457010690))


### Properties

* 'mesh' - 3D mesh
* 'using_tableau' - true if an item (usually body armor) has banner/sigil on it
* 'is_merchandise' - is used to determine whether an item is a merchandise or not. Used in the game code to determine if Item can be lost, on deciding rewards and something else
* 'appearance' - used in calculating the item value (CalculateValue): return (int)(num2 * num * (1f + 0.2f * (item.Appearance - 1f)) + 100f * MathF.Max(0f, item.Appearance - 1f));
* 'value' - price
* 'weight'
* 'culture' (optional) - Defined culture of the current item. If an item is set for a culture, the item will only be sold on the markets of the same culture. Can't have multiple values of the 'culture' attribute, so that the product is available for sale in towns of only two cultures. You can set it to neutral culture, but then it will show up everywhere. 
* 'difficulty' - (need confirmation) is used to indicate the level of difficulty in crafting an item
* 'item_category' - (need confirmation) influences price range based on the market. Example - if item_category="jewelry" and item value="1000", in-game item can cost from ~1000 at start and drop to 150 after few weeks. Because jewelry prices drop in the region. If this item_category is changed to let's say 'book', then in-game same item costs ~1000 again.
* 'subtype' - is used to provide additional information about the type of item being defined. The subtype property is often used in conjunction with the item_category property to further refine the classification of an item
* 'covers_body' - true/false, property for an armor to tell if body should be hidden (true) or not (false)

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


### Broken Item

!!! quote "[PlebeianKilo [ER]](https://discord.com/channels/411286129317249035/697071240015380500/1215514757331943424):"

Since 1.2.x, if a single item is broken, everything past that won't get loaded 

So lets say you have a list of 100 items. If item 32 is broken, everything after doesn't appear in game.

If you want to more easily find out which it is check your logs in programdata (its a hidden folder so you gotta make hidden folders visible) and search for null or invalid.

![](/pics/2403080814.png)


### Transparent Body

![](/pics/202507070944.png)

Use `covers_body="false"` in items.xml for that item


<br>
## How to find ItemID

Let's say we want to find ItemID for this item:

![](/pics/hkirJAc.png)

Easy way: use this [mod](https://www.nexusmods.com/mountandblade2bannerlord/mods/4030)

Hard way: Use [ripgrep](/resources/tools/) in the game's folder:

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
875-        subtype="head_armor"
876-        mesh="battania_helmet_a"
877-        culture="Culture.battania"
878-        weight="0.5"
879-        difficulty="0"
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

### ...and mesh

Mesh (3D model's name) usually is different from the ItemID, and you can find it in the item's definition in the XML:

``` xml
876-        mesh="battania_helmet_a"
```

You will need it for export from [TpacTool](/resources/tpactool/) and working with 3D program such as Blender.

## Remove Native Items

Example [here](/modding/xml/#example-deleteremove-native-items)

## Helmet Hair Coverage

![](/pics/Iq2hAG0.png)

## Beard Coverage

![](/pics/2507120905.png)
