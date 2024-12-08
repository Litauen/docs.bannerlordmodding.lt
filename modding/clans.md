# Clans

[API 1.2.7](https://apidoc.bannerlord.com/v/1.2.7/class_tale_worlds_1_1_campaign_system_1_1_clan.html){target=_blank}

    Clan.PlayerClan


## Tier

    int Tier [get]

## Members

    Clan.Heroes         // alive and dead

## Renown

    Clan.Renown

    AddRenown(float value, bool shouldNotify = true)
    ResetClanRenown()

By [hero](/modding/heroes/#renown)

## Relation

```cs
int FactionManager.GetRelationBetweenClans(Clan clan1, Clan clan2)
```

## Gold

Clan leader's gold

??? example "int         Gold [get]"
    ``` cs
    public int Gold
    {
        get
        {
            Hero leader = this.Leader;
            if (leader == null)
            {
                return 0;
            }
            return leader.Gold;
        }
    }
    ```


## XML

    spclans.xml

### Minor Faction's spawn Location

``` xml
initial_posX="360.0"
initial_posY="212.0"
```

### .IsNomad .IsMafia .IsSect

These properties change dialog lines for a different answer. Also impact hero name and Encyclopedia entries. Nothing else.

Can be set in XML as 'is_nomad'

??? example "dnSpy output"

    ![](/pics/10TEfnN.png)

    ![](/pics/1ZxDEdK.png)

    ![](/pics/m8aHPFU.png)
