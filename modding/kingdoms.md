# Kingdoms

* [API 1.2.7](https://apidoc.bannerlord.com/v/1.2.7/class_tale_worlds_1_1_campaign_system_1_1_kingdom.html)
* [Kingdom Policies](https://mountandblade2bannerlord.wiki.fextralife.com/Kingdom+Management)
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

