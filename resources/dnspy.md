# dnSpy

.NET debugger and assembly editor

## Download

* [Original discontinued version](https://github.com/dnSpy/dnSpy/releases)
* [Unofficial revival](https://github.com/dnSpyEx/dnSpy/releases)

## Guides

* [How to DEBUG Bannerlord](https://www.nexusmods.com/mountandblade2bannerlord/mods/2667)
* [Read the Game Code with dnSpy](https://www.youtube.com/watch?v=SUBcBx9WWgA&list=PLzebdAxJeltRwfJ8jzsNolgHkRvLjoCRC&index=8)


## Setup

* [Setup video](https://youtu.be/SUBcBx9WWgA?list=PLzebdAxJeltRwfJ8jzsNolgHkRvLjoCRC)


## How to attach dnSpy to Bannerlord and catch the exception

* Launch the Bannerlord launcher (not the game itself!)
* Run dnSpy
* In dnSpy, choose the command Debug - Attach to process...
* Select the process corresponding to Bannerlord, then click Attach to confirm
* If all goes well, you will see a "Running..." status confirming that dnSpy is attached to the Bannerlord process
* Start the game. Play until a critical error occurs
* You will see an error message on the screen

[Source with pictures here by LogRaam](https://www.nexusmods.com/mountandblade2bannerlord/mods/2667)


## Not working breakpoints

![](/pics/2402181413.png)

In the module's .csproj file make sure you have

``` xml
<TargetFramework>net472</TargetFramework>
```

and not

``` xml
<TargetFramework>net472,netstandard2.0</TargetFramework>
```

or anything else. Recompile and should be ok.


## Crash on the game start

Crash only when attached to the dnSpy.

![](/pics/2403121926.png)

The cause is RTS Camera mod. Disable it and game will start.