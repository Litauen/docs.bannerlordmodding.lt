# Popups / Messages

## Tutorials

* [Custom Popup with Image](/guides/custom_popup)

## DisplayMessage

``` cs
InformationManager.DisplayMessage(new InformationMessage("message", TaleWorlds.Library.Color.ConvertStringToColor("#FF0042FF")));
```

![](https://imgur.com/1BK27yg.png)

## ShowInquiry

``` cs
InformationManager.ShowInquiry(new InquiryData("Title", "Text", true, true, "AffirmativeText", "NegativeText", null, null, "event:/ui/notification/peace", 0f, null), true, false);
```

![](https://i.imgur.com/y2cp4rK.png)


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

![](https://i.imgur.com/wgSyy4u.png)


## ShowMultiSelectionInquiry

``` cs
MultiSelectionInquiryData(
  string titleText,
  string descriptionText,
  List<InquiryElement> inquiryElements,
  bool isExitShown,
  int maxSelectableOptionCount,
  string affirmativeText,
  string negativeText,
  Action<List<InquiryElement>> affirmativeAction,
  Action<List<InquiryElement>> negativeAction,
  string soundEventPath = "")

MBInformationManager.ShowMultiSelectionInquiry(MultiSelectionInquiryData data);
```

![](https://i.imgur.com/zxK9WCD.png)


## AddQuickInformation

``` cs
MBInformationManager.AddQuickInformation(content, 0, announcer, sounEventPath);
```

![](https://imgur.com/hHlWSBu.png)