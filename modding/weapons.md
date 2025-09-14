# Weapons

* [Official doc](https://moddocs.bannerlord.com/asset-management/weapon_smithing/){target=_blank}
* [What is weapon handling?](https://www.gurugameguides.com/post/understanding-weapon-handling-in-mount-blade-ii-bannerlord)

Weapons are constructed from:

 * Crafting Templates `/Native/ModuleData/crafting_templates.xml`
 * Crafting Pieces `/Native/ModuleData/crafting_pieces.xml`
 * Weapon Descriptions `/Native/ModuleData/weapon_descriptions.xml`
 * Crafted Items `/SandBoxCore/ModuleData/items/weapons.xml`


## Crafting Templates

![](/pics/2509132127.png)

Crafting Template is a 'recipe' how some type of weapon is made, from which parts, what stats it can have, etc

Templates define what tabs your pieces appear in the smithing menu.

Described in `/Native/ModuleData/crafting_templates.xml`

??? abstract "These are Crafting Templates:"
    * Dagger
    * Javelin
    * Mace
    * One Handed Axe
    * One Handed Sword
    * Pike
    * Throwing Axe
    * Throwing Knife
    * Two Handed Axe
    * Two Handed Mace
    * Two Handed Polearm
    * Two Handed Sword



## Crafting Pieces

<center>![](/pics/2509132134.png)</center>

Crafting Pieces are the 'ingredients' (parts) from which weapons by their 'recipes' (Crafting Templates) are made.

Defined in `/Native/ModuleData/crafting_pieces.xml`





Adding is_default="true" to craftingPiece in XML makes it unlocked in the new game.


### Make civilian

In crafting_pieces:

    <Flag name="Civilian" type="ItemFlags" />

!!! failure "&lt;Flags Civilian="true" /> in the &lt;Items>&lt;CraftedItem> does not work"


## Weapon Descriptions

Defined in `/Native/ModuleData/weapon_descriptions.xml`

Weapon description is used for defining different usages and features. 

Weapon Descriptions determine which ways you can use the weapon (although limited by what's available in the template) such as a two handed sword being able to be used as 1 handed as well. Which style of weapons, which style of block, etc.

Defined parts listed under the crafting templates nodes must also be listed under the `weapon_descriptions.xml` to be used.

??? abstract "Weapon Descriptions:"

    * OneHandedSword
    * OneHandedBastardSword
    * OneHandedBastardSwordAlternative
    * TwoHandedSword
    * Dagger
    * ThrowingKnife
    * OneHandedAxe
    * OneHandedBastardAxe
    * OneHandedBastardAxeAlternative
    * TwoHandedAxe
    * ThrowingAxe
    * OneHandedPolearm
    * OneHandedPolearm_JavelinAlternative
    * TwoHandedPolearm
    * TwoHandedPolearm_Couchable
    * TwoHandedPolearm_Pike
    * TwoHandedPolearm_Bracing
    * TwoHandedPolearm_Thrown
    * Javelin
    * Mace
    * TwoHandedMace



## Crafted Items

The actual weapon.

Defines a pre-defined weapon that can appear in shops or the cheat inventory instead of just being craftable.

Defined in `/SandBoxCore/ModuleData/items/weapons.xml`


## Check active weapon

```cs

EquipmentIndex wieldedWeaponIndex = affectedAgent.GetWieldedItemIndex(Agent.HandIndex.MainHand);
if (wieldedWeaponIndex != EquipmentIndex.None)
{
    EquipmentElement wieldedWeapon = defender.BattleEquipment[wieldedWeaponIndex];

    SkillObject skill = DefaultSkills.OneHanded;
    if (wieldedWeapon.Item.PrimaryWeapon.IsPolearm)
    {
        skill = DefaultSkills.Polearm;
    } else if (wieldedWeapon.Item.PrimaryWeapon.IsTwoHanded) {
        skill = DefaultSkills.TwoHanded;
    }

    defender.HeroDeveloper.AddSkillXp(skill, 25, true, true);
}
```


## Use on horseback

Q. what exactly decides that a weapon is not usable on horseback?

A. `requires_no_mount` flag in `item_usage_sets.xml`


## Weapon Class

``` cs
public enum WeaponClass
{
    Undefined,
    Dagger,
    OneHandedSword,
    TwoHandedSword,
    OneHandedAxe,
    TwoHandedAxe,
    Mace,
    Pick,
    TwoHandedMace,
    OneHandedPolearm,
    TwoHandedPolearm,
    LowGripPolearm,
    Arrow,
    Bolt,
    Cartridge,
    Bow,
    Crossbow,
    Stone,
    Boulder,
    ThrowingAxe,
    ThrowingKnife,
    Javelin,
    Pistol,
    Musket,
    SmallShield,
    LargeShield,
    Banner,
    NumClasses
}
```



## Force agents to use couch lance

!!! quote "Info by Eagle:"

The goal is to disable weapon usage for an agent under certain conditions. The key lies in using the `requires_mount` and `requires_no_mount` flags in an `item_usage_set`.

By creating custom variants of the game's item_usage_sets with these flags, you can dynamically assign them to a MissionWeapon. To do this, you can use reflection like so:

```cs
private static readonly MethodInfo? ItemUsageSetter = typeof(WeaponComponentData).GetMethod(
    "set_ItemUsage",
    BindingFlags.Instance | BindingFlags.NonPublic | BindingFlags.DeclaredOnly
);
```

Then, apply it like this:

```cs
MissionWeapon missionWeapon = agent.Equipment[EquipmentIndex.WeaponItemBeginSlot];
ItemUsageSetter.Invoke(weapon.CurrentUsageItem, new object[] { "item_usage_set_id" });
```

If a mounted agent has a one-handed weapon and a lance, you can "disable" the one-handed weapon by creating a custom variant of the onehanded_block_shield_swing_thrust usage set and adding the requires_no_mount flag. Assigning this usage set to the one-handed weapon's MissionWeapon will make it unusable while mounted.

Similarly, you can create a lance usage set that only allows passive couching by removing other attacks. Assigning this to the lance forces the AI to couch lance as there is no other kind of attack.

To apply the changes, you can use:

```cs
agent.EquipWeaponWithNewEntity(EquipmentIndex.WeaponItemBeginSlot, ref missionWeapon);
```


Moreover, you can disable weapons for agents on foot by applying the requires_mount flag in the usage set variant. 

However, this method requires you to manage AI weapon logic during battles. It’s experimental and might not work perfectly in all scenarios. But I used this approach successfully to force AI to couch lances for a jousting tournament.


## Weapon LODs

Info [here](/3d/create_lods/#weapon-lods)


## Construct weapons

* Open all crafting pieces in console (Alt + \`) with `campaign.unlock_all_crafting_pieces`
* Go to the smithy
* Craft the weapon you like
* CTRL+c - copy the code to the Clipboard

??? abstract "Looks like this:"
    ``` xml
    <CraftedItem id="1323A59FC8F8FE171E19E3A88E5EA864"
                             name="Crafted One Handed Sword"
                             crafting_template="OneHandedSword">
    <Pieces>
        <Piece id="aserai_blade_18" Type="Blade"/>
        <Piece id="aserai_guard_1" Type="Guard"/>
        <Piece id="sickle_grip_1" Type="Handle"/>
        <Piece id="aserai_pommel_2" Type="Pommel"/>
    </Pieces>
    <!-- Length: 75 Weight: 0.72 -->
    </CraftedItem>
    ```

* Change `id` and `name`, add `culture`, `is_merchandise` tags (and others if necessary)
* Use the weapon code in XML file (include in your mod within <Items> type file)

??? abstract "Example"

    /MOD_FOLDER/SubModule.xml

    ```xml
    <XmlNode>
        <XmlName id="Items" path="items"/>
        <IncludedGameTypes>
            <GameType value = "Campaign"/>
            <GameType value = "CampaignStoryMode"/>
            <GameType value = "CustomGame"/>
            <GameType value = "EditorGame"/>
        </IncludedGameTypes>
    </XmlNode>
    ```

    /MOD_FOLDER/ModuleData/items/my_custom_daggers.xml

    ``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <Items>
        <CraftedItem id="lt_baltic_war_knife" name="{=}Baltic War Knife" crafting_template="Dagger" culture="Culture.baltic" is_merchandise="true">
            <Pieces>
                <Piece id="dagger_blade_1" Type="Blade" scale_factor="107"/>
                <Piece id="sturgian_dagger_guard_6" Type="Guard" scale_factor="80"/>
                <Piece id="sturgian_grip_12" Type="Handle" scale_factor="106"/>
                <Piece id="cleaver_pommel_4" Type="Pommel" scale_factor="90"/>
            </Pieces>
            <!-- Length: 50 Weight: 0.54 -->
        </CraftedItem>
    </Items>
    ```


!!! note "You can use CTRL+v to paste XML code back to the Smithy and see the weapon from the code"