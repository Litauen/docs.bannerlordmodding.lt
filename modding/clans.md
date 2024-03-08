# Clans

[API 1.2.7](https://apidoc.bannerlord.com/v/1.2.7/class_tale_worlds_1_1_campaign_system_1_1_clan.html){target=_blank}

    Clan.PlayerClan



## Tier

    int Tier [get]


## .IsNomad .IsMafia .IsSect

These properties change dialog lines for a different answer. Also impact hero name and Encyclopedia entries. Nothing else.

Can be set in XML as 'is_nomad'

??? example "dnSpy output"

    ![](/pics/10TEfnN.png)

    ![](/pics/1ZxDEdK.png)

    ![](/pics/m8aHPFU.png)


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
