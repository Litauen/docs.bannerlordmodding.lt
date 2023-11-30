# Console

 * [Commands](https://www.radiotimes.com/technology/gaming/bannerlord-cheats-codes-console-commands/){target=_blank}
 * [VIDEO How to use](https://www.youtube.com/watch?v=2WJcRFJmX0k){target=_blank} by Strat Gaming

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

    campaign.change_hero_relation -50 Olek

GUI Layers

    ui.set_screen_debug_information_enabled true


## Some other commands

- campaign.add_renown_to_clan #
- campaign.add_gold_to_hero #
- campaign.give_troops help
- battanian_fian_champion #
- khuzait_khans_guard #
- campaign.give_item_to_main_party ItemName #
- campaign.add_attribute_points_to_hero #
- campaign.add_focus_points_to_hero #
- campaign.set_all_skills_main_hero #
- campaign.add_companions #
- campaign.set_all_companion_skills #
- campaign.give_all_crafting_materials_to_main_party #
- campaign.unlock_all_crafting_pieces
- campaign.give_settlement_to_player Town/castle
- campaign.marry_player_with_hero HeroName
- campaign.kill_hero HeroName
- campaign.join_kingdom KingdomName
- campaign.lead_your_faction
- campaign.add_influence #
- campaign.multiply_campaign_speed #
- atmosphere.set_interpolation_tod 120
- campaign.move_time_forward # (hours)
- campaign.set_main_party_attackable 0
- campaign.declare_war faction1 faction2
- campaign.declare_peace faction1 faction2

NOTE: faction1/faction2 can be your clan's name

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

