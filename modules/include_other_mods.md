# Include other modules when developing

In the file YOUR_MODULE/Properties/launchSettings.json add the necessary modules.

Example to include BannerKings with all necessary modules:

    "commandLineArgs": "/singleplayer _MODULES_*Bannerlord.Harmony*Bannerlord.ButterLib*Bannerlord.UIExtenderEx*Bannerlord.MBOptionScreen*Native*SandBoxCore*CustomBattle*Sandbox*StoryMode*RBM*OpenSourceArmory*BannerKings*$(ModuleName)*_MODULES_",