# MBSubModuleBase

[API](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_mount_and_blade_1_1_m_b_sub_module_base.html){target=_blank}

Method|Comment
------|-------
OnSubModuleLoad|(First) Starts as soon as the mod is loaded. Called during the first loading screen of the game, always the first override to be called, this is where you should be doing the bulk of your initial setup
OnSubModuleUnloaded|Called when exiting Bannerlord entirely
OnBeforeInitialModuleScreenSetAsRoot|(Second) Starts when the first loading screen is done. Called just before the main menu first appears, helpful if your mod depends on other things being set up during the initial load
OnGameStart|(Third) Starts as soon as you load a game. Called immediately upon loading after selecting a game mode (submodule) from the main menu
BeginGameStart|(Fourth) Starts when the game starts loading. Called immediately after loading the selected game mode (submodule) has completed
OnGameLoaded|(Fifth) Starts when the game finishes loading. Called only after loading a save
OnNewGameCreated|Starts when a new Campaign or Sandbox game is created.
OnCampaignStart|Starts when you press the Campaign Button on the main screen. Called once any game mode is started
OnGameInitializationFinished|(Last) Starts after the game finishes initializing. Called once the initialization for a game mode has finished
OnAfterGameInitializationFinished|
DoLoading|Called seemingly as loading is ending, not entirely sure of this one
OnGameEnd|Called on exiting out of a mission/campaign
OnApplicationTick|This is called once every frame, you should avoid expensive operations being called directly here and instead do as little work as possible for performance reasons
OnBeforeMissionBehaviorInitialize|
OnMissionBehaviorInitialize|
OnConfigChanged|
AfterAsyncTickTick|
InitializeGameStarter|
OnInitialState|
OnMultiplayerGameStart|
RegisterSubModuleObjects|
AfterRegisterSubModuleObjects|
