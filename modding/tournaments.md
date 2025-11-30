# Tournaments



Set different templates for 1/2/4 participant in the group matches in SandBoxCore/ModuleData/spculture.xml:

``` xml
<tournament_team_templates_one_participant>
<tournament_team_templates_two_participant>
<tournament_team_templates_four_participant>
```

Templates are defined in SandBoxCore/ModuleData/spnpccharactertemplates.xml

such as:

```xml
<NPCCharacter id="tournament_template_CULTURE_one_participant_set_v1" ...
```

## Prizes

Elite tournament rewards are hardcoded in `FightTournamentGame - CachePossibleEliteRewardItems()`


## In town?

    Campaign.Current.TournamentManager.GetTournamentGame(town) != null




## Tournament Spawn Mechanics:

Code:

    TournamentCampaignBehavior - ConsiderStartOrEndTournament

    DefaultTournamentModel - GetTournamentStartChance


### Town-specific week rotation (deterministic scheduling)

    Math.Abs(town.StringId.GetHashCode() % 3) != CampaignTime.Now.GetWeekOfSeason

Each town is assigned to one of 3 weeks in a season based on its ID hash. Tournaments can only be considered during that specific week, meaning:

Each town has a tournament "window" of ~1 week out of every 3 weeks

Towns are distributed across the 3 weeks to spread tournaments across the map

### Base chance calculation (when in the correct week)

    0.1f * (lordParties + suitableHeroes) - 0.2f

The chance depends on town activity:

* +0.1 per lord party at the settlement
* +0.1 per suitable hero without a party
* -0.2 base penalty

Examples:

* 2 lords, 0 heroes = 0.1 × 2 - 0.2 = 0% chance (too quiet)
* 3 lords, 1 hero = 0.1 × 4 - 0.2 = 20% chance
* 5 lords, 3 heroes = 0.1 × 8 - 0.2 = 60% chance
* 10+ lords/heroes = capped at 80% chance (0.1 × 10 - 0.2)

### Blockers

* Under siege = 0% chance
* Wrong week for this town = 0% chance
* Less than 15 days since last tournament = not even checked

Actual Frequency: For a moderately active town (say 30% chance when checked):

* Checked daily during its 1-week window every 3 weeks
* 15-day minimum cooldown between tournaments

Results in roughly 1-2 tournaments per season per town in practice

