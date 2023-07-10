# Missions

[TaleWorlds.MountAndBlade.Mission Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_mount_and_blade_1_1_mission.html){target=_blank}

Where you control your character in 3D environment. Not a global map. Examples: Town, village, siege, battles, hideouts, prison break


    MissionBehaviour (PURE MissionBehavior not present in the game)

    MissionView - subtype of MissionBehaviour - views, icons, scores, etc
        shows some UI to the player
        handles input
        listens to keypress

    MissionLogic - subtype of MissionBehaviour - manages the mission in a some way
        spawning agents
        handling reinformcements
        handling routing

