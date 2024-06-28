# Advanced Stacktrace Analytics of Crash Reports

[Source](https://www.nexusmods.com/mountandblade2bannerlord/articles/674) by [BUTR] Aragas


## Introduction

ButterLib v2.2.4 added a new entry in its Crash Report section called Enhanced Stacktrace.

It provides more info about the stacktrace of the exception - every frame (method call) provides more info:

* If the method is changed by Harmony, it shows the original method and any prefix, postfix, transpiler or finalizer that is added, included with the Module that introduces the patch;
* An IL Offset is added;


## IL Offsets

You should be able to know now the line that caused the exception, even if no debug symbols were present at the time of the crash.

This will be the most helpful when debugging vanilla game code.

Here's how to use it:

* Download dnSpy or any other software that can show the IL code of a method;
* Convert the IL Offset to hexadecimal;
* Open the IL Code of the method;
* The IL Offset will link to the exact line the code threw an exception;

## Example

As an example, here's a [crash report](https://report.butr.link/8EA037.html).


```
Frame: InitCraftedItemObject at offset 207 in file:line:column :0:0 (IL Offset: 73)
Module: UNKNOWN
Method: static System.Void TaleWorlds.Core.ItemObject::InitCraftedItemObject(TaleWorlds.Core.ItemObject& itemObject,
 TaleWorlds.Localization.TextObject name, TaleWorlds.Core.BasicCultureObject culture, TaleWorlds.Core.ItemFlags itemProperties,
 System.Single weight, System.Single appearance, TaleWorlds.Core.WeaponDesign craftedData, TaleWorlds.Core.ItemTypeEnum itemType)
```

The IL Offset of the method InitCraftedItemObject is 73 (0x49 hex).


![](/pics/2406272114.png)

The IL code shows that at 0049 is the line

    itemObject.ItemHolsters = (string[])craftedData.Template.ItemHolsters.Clone();


![](/pics/2406272115.png)

On which the NullReferenceException occured.