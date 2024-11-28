# Locations

## Add character to the location

``` cs
Location tavern = LocationComplex.Current.GetLocationWithId("tavern");
locationCharacter.SpecialTargetTag = "sp_notable";   // enforce where to place it
tavern.AddCharacter(locationCharacter);
```

If character is in another location, it is removed from the old one. No duplicates are created.

## Town

    * center
    * arena
    * tavern
    * lordshall
    * prison
    * house_1
    * house_2
    * house_3

## Castle

    * center
    * lordshall
    * prison

## Village

    * village_center


