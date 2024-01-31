# Popups / Messages

## Tutorials

* [Custom Popup with Image](/guides/custom_popup)

## DisplayMessage

``` cs
InformationManager.DisplayMessage(new InformationMessage("message", TaleWorlds.Library.Color.ConvertStringToColor("#FF0042FF")));
```

![](/pics/1BK27yg.png)

## ShowInquiry

``` cs
InformationManager.ShowInquiry(new InquiryData("Title", "Text", true, true, "AffirmativeText", "NegativeText", null, null, "event:/ui/notification/peace", 0f, null), true, false);
```

![](/pics/y2cp4rK.png)


## ShowTextInquiry

``` cs
TextInquiryData(
    string titleText,
    string text,
    bool isAffirmativeOptionShown,
    bool isNegativeOptionShown,
    string affirmativeText,
    string negativeText,
    Action<string> affirmativeAction,
    Action negativeAction,
    bool shouldInputBeObfuscated = false,
    Func<string, Tuple<bool, string>> textCondition = null,
    string soundEventPath = "",
    string defaultInputText = "")

ShowTextInquiry(
    TextInquiryData textData,
    bool pauseGameActiveState = false,
    bool prioritize = false)

InformationManager.ShowTextInquiry(new TextInquiryData("Title", "Text", true, true, "AffirmativeText", "NegativeText", null, null, false, null, "", "defaultInputText"), false, false);
```

![](/pics/wgSyy4u.png)


## ShowMultiSelectionInquiry

``` cs
MultiSelectionInquiryData(
  string titleText,
  string descriptionText,
  List<InquiryElement> inquiryElements,
  bool isExitShown,
  int minSelectableOptionCount,
  int maxSelectableOptionCount,
  string affirmativeText,
  string negativeText,
  Action<List<InquiryElement>> affirmativeAction,
  Action<List<InquiryElement>> negativeAction,
  string soundEventPath = "")

MBInformationManager.ShowMultiSelectionInquiry(MultiSelectionInquiryData data);
```

![](/pics/zxK9WCD.png)


## AddQuickInformation

![](/pics/dRrDaYF.png)

``` cs
MBInformationManager.AddQuickInformation(TextObject content, int priority, CharacterObject announcer, string sounEventPath);
```

Priority determines how long the message stays, approximate values:

- 0 - 2.5s
- 1000 - 3.5s
- 5000 - 7.5s
- 10000 - 13s
