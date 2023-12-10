# Settlement Position Script

Find in the lower bottom corner of the map or in the Entities filter.

![](https://imgur.com/Vo8Iwc0.png)


It contains a SettlementPositionScript with buttons to CheckPositions, SavePositions & ComputeAndSaveSettlementDistanceCache.
The game relies on caching distances between settlements for AI pathing and decision making. Accordingly, I believe that every worldmap needs such a script entity to work in game. ([src](https://docs.google.com/document/d/1npGJ9p1ySdu2RDU19P_2aE-OCsKWie_G02vcws36UIs/edit))

## CheckPositions

![](https://imgur.com/QyEK1Xk.png)


### Workflow

!!! bug "Crashes when there are no more errors?"


1. Load last good map
2. Press on the CheckPositions
3. Crash? All good :), no badly placed settlements, load again and proceed with other tasks. THE END
4. Jumps to some settlement? Move the settlement or adjust the Navmesh
5. SAVE YOUR MAP! (better to some other location, as 'Backup' for example)
6. Press on the CheckPositions
7. Crash? GOTO 1.
8. No crash? GOTO 4.


### Outside Navmesh

![](https://imgur.com/J7ZiQXK.png)


### Inaccessible terrain

![](https://imgur.com/Ca4b2IN.png)



## SavePositions

It updates the locations of the settlement's and gate positions of settlements in the settlements.xml if you moved around any of the corresponding entities

This update is done on map save also.


## ComputeAndSaveSettlementDistanceCache

Does what it tells with [settlements_distance_cache.bin](/modding/settlements/#settlement-distance-cache)
