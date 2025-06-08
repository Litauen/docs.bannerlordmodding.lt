# Modding Dialogs

## Tutorials

* [Creating Hero Dialogue / Conversations](https://forums.taleworlds.com/index.php?threads/creating-hero-dialogue-conversations.450349/){target=_blank}
* [Modding Conversations - research (OUTDATED?)](https://forums.taleworlds.com/index.php?threads/modding-conversations-research.414310/){target=_blank}
* [Danqna and Gispry - Adding dialog with conditions and consequence](https://www.youtube.com/watch?v=ReL_hz1KgzE&list=PL02GptyKz6G0oElRE_27efBWQIE1zsAgd&index=4){target=_blank}
* [Danqna and Gispry - Dialogue in game](https://www.youtube.com/watch?v=Aerqdwf3yAA&list=PL02GptyKz6G0oElRE_27efBWQIE1zsAgd&index=5){target=_blank}
* [Custom Audio with LipSync](/guides/custom_audio_with_lipsync/)





<br>
## Implementation example

``` cs
private void OnSessionLaunched(CampaignGameStarter starter)
{
    AddDialogs(starter);
}


private void AddDialogs(CampaignGameStarter starter)
{

    starter.AddPlayerLine("tavernkeeper_book", "tavernkeeper_talk", "tavernkeeper_book_seller_location", "Have you seen any book vendor recently?", TavernKeeperOnCondition, () => { GiveGoldAction.ApplyBetweenCharacters(null, Hero.MainHero, -5, false); }, 100, (out TextObject explanation) =>
    {
        if (Hero.MainHero.Gold < 5)
        {
            explanation = new TextObject("Not enough gold...");
            return false;
        }
        else
        {
            explanation = new TextObject("5 {GOLD_ICON}");
            return true;
        }
    });

    starter.AddDialogLine("tavernkeeper_book_a", "tavernkeeper_book_seller_location", "tavernkeeper_books_thanks", "Yeah, saw {VENDOR.FIRSTNAME} recently. Look around the town.", () => { return IsBookVendorInTown(); }, null, 100, null);
    starter.AddDialogLine("tavernkeeper_book_a", "tavernkeeper_book_seller_location", "tavernkeeper_books_thanks", "I heard you can find what you are looking for in {SETTLEMENT}.", () => { return IsBookVendorNearby(); }, null, 100, null);
    starter.AddDialogLine("tavernkeeper_book_b", "tavernkeeper_book_seller_location", "tavernkeeper_books_thanks", "No, haven't heard lately.", null, null, 100, null);

    starter.AddPlayerLine("tavernkeeper_book", "tavernkeeper_books_thanks", "tavernkeeper_pretalk", "Thanks!", null, null, 100, null, null);

    starter.AddDialogLine("tavernkeeper_book", "tavernkeeper_pretalk", "tavernkeeper_talk", "Anything else?", null, null, 100, null);
}

// Condition example
private bool IsBookVendorInTown()
{
    foreach (Hero vendor in _vendorList)
    {
        if (vendor.CurrentSettlement == Settlement.CurrentSettlement)
        {
            StringHelpers.SetCharacterProperties("VENDOR", vendor.CharacterObject, null, false);
            return true;
        }
    }
    return false;
}

```

Diagram of the example above:

![](/pics/pC7cbU1.png)


<br>
## AddPlayerLine

    What player (you) says

[AddPlayerLine](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_campaign_game_starter.html#abc007cf84183d0f1a73e0c28a65ba90e){target=_blank}(id, inputToken, outputToken, text, condition, consequence, priority, clickableCondition, persuasionOption)

* id - just a name for the entry, not used anywhere?
* inputToken - entry name, where we can come from another dialog line
* outputToken - where to go from here, to another dialog's line entry (inputToken).  outputToken --> inputToken
* text - what to show
* condition - on what condition to show this dialog line (can be several, true - show, false - do not show, no condition -> show if no other conditions or other conditions false, hide if more conditions and one of it's true)
* priority - default 100, if you want to overwrite default dialog, make it greater, like 110 and use same id/inputToken
* clickableCondition - possible to disable with the explanation (example follows)
* persuasionOption - something about the persuasions, consult with dnSpy :grin:

### Disable with Explanation

<center>
![](/pics/1taJpnI.png)
</center>

Disable the PlayerLine with the Explanation like this:

``` cs
starter.AddPlayerLine("tavernkeeper_book", "tavernkeeper_talk", "tavernkeeper_book_seller_location", "{TAVERN_KEEPER_GREETING}", TavernKeeperOnCondition, () => { GiveGoldAction.ApplyBetweenCharacters(null, Hero.MainHero, -5, false); }, 100, (out TextObject explanation) =>
{
    if (Hero.MainHero.Gold < 5)
    {
        explanation = new TextObject("Not enough gold...");
        return false; // line is disabled
    }
    else
    {
        explanation = new TextObject("5 {GOLD_ICON}");
        return true; // line is enabled
    }
});

```


<br>
## AddDialogLine

    What NPC says

[AddDialogLine](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_campaign_game_starter.html#ada839c7e59fedd1555ec1fbb160a8875){target=_blank}(id, inputToken, outputToken, text, condition, consequence, priority, clickableCondition)

Parameters same as in AddPlayerLine.

InputToken -  "start" on first dialog entry point and it needs a start condition.

The last line should have outputToken "end" if you want the dialog to end:

``` cs
starter.AddDialogLine("bookvendor_talk", "bookvendor_talk", "end", "Bye", null, null, 100, null);
```

!!! danger "If you will use outputToken "end" with AddPlayerLine - the game will hang"

Also it can be ''close_window'' (for notables at least)
``` cs
starter.AddDialogLine("village_build_job", "village_build_job_options_selected", "close_window", "Great, let's start.", null, null);
```


<br>
## Story demo

<center>
<video width="640" height="360" controls autoplay loop muted>
    <source src="/pics/dialog_story_demo.webm" type="video/webm">
    Your browser does not support the video tag.
</video>
</center>


``` cs
starter.AddPlayerLine("tavernkeeper_story", "tavernkeeper_talk", "tavernkeeper_story_part1", "Tell me a story", null, null, 100, null, null);
starter.AddDialogLine("tavernkeeper_story", "tavernkeeper_story_part1", "tavernkeeper_story_part2", "STORY PART 1", null, null, 100, null);
starter.AddDialogLine("tavernkeeper_story", "tavernkeeper_story_part2", "tavernkeeper_story_part3", "STORY PART 2", null, null, 100, null);
starter.AddDialogLine("tavernkeeper_story", "tavernkeeper_story_part3", "tavernkeeper_story_part_end", "STORY PART 3", null, null, 100, null);
starter.AddDialogLine("tavernkeeper_story", "tavernkeeper_story_part_end", "tavernkeeper_story_part_end_player", "THE END", null, null, 100, null);
starter.AddPlayerLine("tavernkeeper_story", "tavernkeeper_story_part_end_player", "tavernkeeper_story_part_end_teller", "Thank you. That was quite a story", null, null, 100, null, null);
starter.AddDialogLine("tavernkeeper_story", "tavernkeeper_story_part_end_teller", "tavernkeeper_talk", "I am glad you liked it.", null, null, 100, null);
```


<br>
## [AddRepeatablePlayerLine](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_campaign_game_starter.html#af1cd557ea251cdfd217648c6871b4921){target=_blank}

    Dynamic method to add several options to choose from.

``` cs
AddRepeatablePlayerLine
    (string id,
    string inputToken,
    string outputToken,
    string text,
    string continueListingRepeatedObjectsText,
    string continueListingOptionOutputToken,
    ConversationSentence.OnConditionDelegate conditionDelegate,
    ConversationSentence.OnConsequenceDelegate consequenceDelegate,
    int priority = 100,
    ConversationSentence.OnClickableConditionDelegate clickableConditionDelegate = null)
```


Example in which we can choose what fief to give to our companion:

``` cs
campaignGameStarter.AddRepeatablePlayerLine("turn_companion_to_lord_has_fief_list", "player_has_fief_list", "player_selected_fief_to_grant", "{=3rHeoq6r}{SETTLEMENT_NAME}.", "{=sxc2D6NJ}I am thinking of a different location.", "check_player_has_fief_to_grant", new ConversationSentence.OnConditionDelegate(CompanionRolesCampaignBehavior.list_player_fief_on_condition), new ConversationSentence.OnConsequenceDelegate(this.list_player_fief_selected_on_consequence), 100, new ConversationSentence.OnClickableConditionDelegate(CompanionRolesCampaignBehavior.list_player_fief_clickable_condition));

private static bool list_player_fief_on_condition()
{
    Settlement settlement = ConversationSentence.CurrentProcessedRepeatObject as Settlement;
    if (settlement != null)
    {
        ConversationSentence.SelectedRepeatLine.SetTextVariable("SETTLEMENT_NAME", settlement.Name);
    }
    return true;
}
```

<br>
## [AddDialogLineWithVariation](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_campaign_game_starter.html#af9650b1acdc96eafed05dad01432080a){target=_blank}


??? exmaple "Different answers based on character traits for example:"

    From LordConversationsCampaignBehaviour

    ``` cs
    starter.AddDialogLineWithVariation("player_turns_down_surrender", "party_encounter_lord_hostile_attacker_3", "close_window", null, null, 100, "", "", "", "", null).Variation(new object[]
    {
        "{=QWzGkQrT}So we fight, then.[if:idle_angry][ib:warrior]",
        "DefaultTag",
        1
    }).Variation(new object[]
    {
        "{=6i7a1c4E}Very well. Death before dishonor![if:idle_angry][ib:warrior]",
        "PersonaEarnestTag",
        1,
        "ChivalrousTag",
        1
    }).Variation(new object[]
    {
        "{=FMJuPZlm}I'm not surrendering, so do what you must.[if:idle_angry][ib:warrior]",
        "ChivalrousTag",
        1
    }).Variation(new object[]
    {
        "{=ZG6kWWwW}I'm not yielding, so let's go to it, then.[if:idle_angry][ib:warrior]",
        "PersonaCurtTag",
        1
    }).Variation(new object[]
    {
        "{=SPYDUXvx}We meet on the battlefield, then.[if:idle_angry][ib:warrior]",
        "PersonaSoftspokenTag",
        1
    }).Variation(new object[]
    {
        "{=3WA4MLzx}One way or the other, you'll regret this. If I fall, my people will have their revenge.[if:idle_angry][ib:warrior]",
        "UncharitableTag",
        1
    }).Variation(new object[]
    {
        "{=yzzY4uXN}Very well. Expect no mercy.[if:idle_angry][ib:warrior]",
        "CruelTag",
        1,
        "FriendlyRelationshipTag",
        -1
    });
    ```

<br>
## [AddDialogLineMultiAgent](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_campaign_game_starter.html#ac92d315373b3a19ed5fe81c570abaca8){target=_blank}

No examples in the game code...

Just a guess that could be used in the dialog with several NPCs.



<br>
## OneToOneConversationHero

Get the Hero you are talking to in the dialog

``` cs
Hero.OneToOneConversationHero;
```


<br>
## Notes

!!! warning "Requires to reload a game for a new dialog change to appear in-game."

- [ ] does not show up in the AddDialogLine. Maybe because of the [if:idle_angry][ib:warrior] tags.

<br>
## String helpers

In ConditionDelegate set variables:

``` cs
StringHelpers.SetCharacterProperties("HERO", hero.CharacterObject, null, false);
MBTextManager.SetTextVariable("GOLD_ICON", "{=!}<img src=\"General\\Icons\\Coin@2x\" extend=\"8\">", false);
MBTextManager.SetTextVariable("GREETING", "Howdy", false);
MBTextManager.SetTextVariable("SETTLEMENT", settlement.EncyclopediaLinkWithName, false);
```

Use in the dialog:

``` cs
starter.AddDialogLine("a", "b", "c", "{GOLD_ICON} {GREETING} {HERO.NAME} in {SETTLEMENT}! {GOLD_ICON}", OnConditionDelegateFunction, null, 100, null);
```

For random text, generate it in the ConditionDelegate which is run on each menu instance.


<br>
## Face/Body NPC Tags

<center>
![](/pics/mbMm9iV.png)
</center>

[Face/Body control tags for NPCs in Dialogs](https://docs.google.com/document/d/1Vm_KkjmtLGOxad4wLBmaj3E9TQtgILacNP4jGNYqgIU/edit#heading=h.5mgttots7rq6){target=_blank}

    Control how NPCs behave during the dialog.


<br>
## Start battle after the dialog

!!! danger "Not tested"

You can try declaring the battle as an action and execute it after the conversation ended like this:

``` cs
.Consequence(delegate { Campaign.Current.ConversationManager.ConversationEndOneShot += YourMethodCall; })
```

