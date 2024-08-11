# Include other modules when developing

In the file YOUR_MODULE/Properties/launchSettings.json add the necessary modules.

Example to include BannerKings with all necessary modules:

```json
{
  "profiles": {
    "Bannerlord": {
      "commandName": "Executable",
      "executablePath": "$(GameFolder)\\bin\\Win64_Shipping_Client\\Bannerlord.exe",
      "commandLineArgs": "/singleplayer _MODULES_*Bannerlord.Harmony*Bannerlord.UIExtenderEx*Native*SandBoxCore*CustomBattle*Sandbox*StoryMode*RBM*OpenSourceArmory*OpenSourceSaddlery*OpenSourceWeaponry*BannerKings*$(ModuleName)*_MODULES_",
      "workingDirectory": "$(GameFolder)\\bin\\Win64_Shipping_Client"
    }
  }
}

```