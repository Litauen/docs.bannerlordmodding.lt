# Weapons

* [Official doc](https://moddocs.bannerlord.com/asset-management/weapon_smithing/){target=_blank}


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