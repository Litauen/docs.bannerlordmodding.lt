# Console

 * [Commands](https://www.radiotimes.com/technology/gaming/bannerlord-cheats-codes-console-commands/){target=_blank}

## Enable

Alt + `

## Commands

Cheat mode:

    config.cheat_mode 1


Teleport:

    CTRL + LMB

Add money to hero:

    campaign.add_gold_to_hero 1000000


Speedup time:

    campaign.multiply_campaign_speed 100 (does not work in 1.0.3)
    campaign.set_campaign_speed_multiplier 15 (works in 1.1.0)


Make hero fugitive:

    campaign.make_hero_fugitive Corena

Kill hero:

    campaign.kill_hero Corena

Change Relation:

    campaign.change_hero_relation Olek -50

GUI Layers

    ui.set_screen_debug_information_enabled true


## Own console command

![](https://i.imgur.com/Bk74f84.png)


``` cs
    public class YOURBehaviour : CampaignBehaviorBase
    {
        private static YOURBehaviour Instance { get; set; }

        private bool _debug = false;

        public YOURBehaviour()
        {
            Instance = this;
        }

        [CommandLineFunctionality.CommandLineArgumentFunction("debug", "custom")]
        public static string ConsoleDebug(List<string> args)
        {
            if (args.Count < 1)
            {
                return "You must provide an argument";
            }

            if (args[0] == "1")
            {
                Instance._debug = true;
                return $"Debug enabled";
            }

            Instance._debug = false;
            return $"Debug disabled";
        }
    }

```

