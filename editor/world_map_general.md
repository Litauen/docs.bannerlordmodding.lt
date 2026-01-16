# World Map

## Custom World Map for War Sails DLC Guide

`(1.3.13)`

!!! quote "Egg:"

* Add all missing stuff from the warsails cultures xslt to all your cultures
* Make new fishing, naval caravan and naval patrol party templates for each culture or use the 6 native ones for your custom cultures
* Incorporate all warsails settlements, clans, kingdom and lords ID's into your modules (may work by removing them all with xslt)
* Change all island castles to port towns, or add bridges or rocky passage ways to make the castles and villages accessible
* Nav Mesh around all shores tile 7 as a border. Nav mesh all ocean either tile 18 for shallow water or 19 for deep water. Nav mesh under bridges tile 25. Nav mesh navigable rivers tile 11. Make sure all holes in nav mesh are filled with an unnavigable tile like 10
* Add blockade script to potential port town entity in the world map scene. Add child entities of port_town (with city port tag), blockade_start and blockade_end to the port town entity
* Save scene, exit tools and delete all port coordinants out of your settlements xml except for one
* Start the game in sandbox mode and when you get to the world map, run the Lt settlement distance cache generator, until it says success and exit game
* Delete the "new" off of your 3 newly generated SDC, add the coordinants back to your next port in your settlement xml, open game and run the LT SCD script over again to add the new ports distance data
* Rinse and repeat until all of your desired ports are incorporated