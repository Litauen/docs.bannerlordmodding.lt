# Create a new Module

* Based on [Lesser Scholar's Tutorial #1](https://youtu.be/gb3YA-1ml7E?t=736){target=_blank}
* Using [BUTR / Bannerlord.Module.Template](https://github.com/BUTR/Bannerlord.Module.Template){target=_blank}

## Install the template

In PowerShell run:

    dotnet new --install Bannerlord.Templates


## Set Environment Variable

![](/pics/OrsHtAd.png)


## Create a Module

In PowerShell:

    cd YOUR_FOLDER_WHERE_YOU_KEEP_ALL_YOUR_MODS
    dotnet new blmodfx --name "YOUR_MOD_NAME_HERE"

## Open in the Visual Studio

Go to newly created module's folder and press on .csproj file



## If Breakpoints and Hot Reload does not work

!!! failure "The breakpoint will not currently be hit. No symbols have been loaded for this document."

To fix in the .csproj file change line:

``` xml
    <TargetFramework>netstandard2.0</TargetFramework>
```

to

``` xml
    <TargetFramework>net472</TargetFramework>
```
