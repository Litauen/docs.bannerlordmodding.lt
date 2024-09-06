# Kingdoms

* [API 1.2.7](https://apidoc.bannerlord.com/v/1.2.7/class_tale_worlds_1_1_campaign_system_1_1_kingdom.html)
* [Custom Kingdom Creation Guide](https://forums.taleworlds.com/index.php?threads/custom-kingdom-creation-guide.435969/)
* [Руководство по созданию королевства](https://commando.com.ua/commando/gmpr/modding-bannerlord/gajdy-po-mododeliju/6080-rukovodstvo-po-sozdaniju-korolevstva.html)

## Notes

Kingdom can exist without owning a Town. Castle is enough. (It was stated somewhere otherwise)

With the existing Culture to create a Kingdom it is necessary to have:

- Castle or Town
- 1 Clan - the owner of the Castle/Town
- 1 Lord - the owner of the Clan/Kingdom

XML info on relations [here](/modding/xml/).

Not necessary:

- Spouse
- Dead lords


## All Kingdoms

``` cs
foreach (Kingdom kingdom in Kingdom.All) { // do smthg }
```

## Strength

    float       TotalStrength [get]


## Policies

* [Kingdom Policies Descriptions](https://mountandblade2bannerlord.wiki.fextralife.com/Kingdom+Management)

Can be set in spkingdoms.xml

``` xml
<policies>
    <policy     id="policy_royal_guard" />
</policies>
```

Full list [here](/modding/cultures/#default_policies)

In the code:

public class DefaultPolicies

    LandTax
    StateMonopolies
    SacredMajesty
    Magistrates
    DebasementOfTheCurrency
    PrecarialLandTenure
    CrownDuty
    ImperialTowns
    RoyalCommissions
    RoyalGuard
    WarTax
    RoyalPrivilege
    Senate
    LordsPrivyCouncil
    MilitaryCoronae
    FeudalInheritance
    Serfdom
    NobleRetinues
    CastleCharters
    Bailiffs
    HuntingRights
    RoadTolls
    Marshals
    CouncilOfTheCommons
    ForgivenessOfDebts
    Citizenship
    TribunesOfThePeople
    GrazingRights
    Lawspeakers
    TrialByJury
    Cantons

Add a policy:
```cs
kingdom.AddPolicy(DefaultPolicies.RoyalGuard);
```

Remove a policy:
```cs
kingdom.RemovePolicy(DefaultPolicies.RoyalGuard);
```

Check if kingdom has policy
```
if (kingdom.HasPolicy((DefaultPolicies.RoyalGuard) {}
if (Kingdom.ActivePolicies.Contains(DefaultPolicies.CastleCharters))) {}
```

## Find a Kingdom

``` cs
public static Kingdom FindKingdomByStringID(string stringID)
{
    foreach (Kingdom kingdom in Kingdom.All)
    {
        if (kingdom.StringId == stringID)
        {
            return kingdom;
        }
    }
    return null;
}
```
