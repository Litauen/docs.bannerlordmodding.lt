# Console

 * [Commands](https://www.radiotimes.com/technology/gaming/bannerlord-cheats-codes-console-commands/){target=_blank}
 * [VIDEO How to use](https://www.youtube.com/watch?v=2WJcRFJmX0k){target=_blank} by Strat Gaming

## Enable

Alt + `

## Commands


| Command | Comment |
|---------|---------|
|config.cheat_mode 1| Turn cheat mode ON |
|CTRL + LMB | Teleport on the world map |
|campaign.add_gold_to_hero 1000000|Add money to hero|
|campaign.multiply_campaign_speed 100| Speedup time (does not work in 1.0.3)|
|campaign.set_campaign_speed_multiplier 15| Speedup time (works in 1.1.0)|
|campaign.make_hero_fugitive Corena|Make hero fugitive|
|campaign.kill_hero Corena|Kill hero|
|campaign.change_hero_relation -50 Olek|Change Relation|
|campaign.start_player_vs_world_war|Player becomes enemy to everybody|
|campaign.start_world_war|Everybody enemy to everybody|
|campaign.add_troops aserai_tribesman \| 200 | Adds troops to the player |
|campaign.print_main_party_position| Print's players coordinates |
|ui.set_screen_debug_information_enabled true| Debug GUI Layers|
|campaign.add_prisoner vlandian_sharpshooter \| 10 | Add prisoners to the player's party
|campaign.toggle_information_restrictions 1 | automatically discover all heroes

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
- campaign.add_modified_item [Item] | [Modifier]<br>
    campaign.add_modified_item Spike Mace | Legendary

NOTE: faction1/faction2 can be your clan's name

## Own console command

![](/pics/Bk74f84.png)


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

## Disable cheat mode

Code by [hunharibo](https://discord.com/channels/411286129317249035/677511186295685150/1271767725487947857)

```cs

[HarmonyPatch("ScriptingInterfaceOfIConfig", "GetCheatMode")]
public static class GetCheatMode_Patch
{
    public static bool Prefix(ref bool __result)
    {
        __result = false; // Always return false
        return false; // Skip the original method
    }
}
```