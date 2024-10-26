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