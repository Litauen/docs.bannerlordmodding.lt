# Weapons

* [Official doc](https://moddocs.bannerlord.com/asset-management/weapon_smithing/){target=_blank}
* [What is weapon handling?](https://www.gurugameguides.com/post/understanding-weapon-handling-in-mount-blade-ii-bannerlord)

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

## Make civilian

In crafting_pieces:

    <Flag name="Civilian" type="ItemFlags" />


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

## Crafting pieces

Adding is_default="true" to craftingPiece in XML makes it unlocked in the new game.


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
