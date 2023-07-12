# Widgets

Screen elements, main building blocks of [Prefabs](/gauntletui/prefabs)

## Links

* [Widgets from unnoficial BL docummentation](https://docs.bannerlordmodding.com/_gauntlet/widget.html){target=_blank}


## Common Predefined Widgets

* ButtonWidget
* ImageWidget
* ListPanel
* RichTextWidget
* ScrollablePanel
* ScrollBarWidget
* TextWidget
* TooltipWidget
* Widget

## Common Predefined Attributes for Widgets

* [WidthSizePolicy / HeightSizePolicy](/gauntletui/widgets/#widthsizepolicy-heightsizepolicy)
* [HorizontalAlignment / VerticalAlignment](/gauntletui/widgets/#horizontalalignment-verticalalignment)
* [MarginLeft / MarginRight / MarginTop / MarginBottom](/gauntletui/widgets/#marginleft-marginright-margintop-marginbottom)
* [PositionXOffset / PositionYOffset](/gauntletui/widgets/#positionxoffset-positionyoffset)
* [ScaledPositionXOffset / ScaledPositionYOffset](/gauntletui/widgets/#scaledpositionxoffset-scaledpositionyoffset)
* [Brush](/gauntletui/brushes)
* [IsVisible / IsHidden](/gauntletui/widgets/#isvisible-ishidden)
* Command.Click (Command.YourKeyHere)
* DataSource (Properties with DataSourceProperty Attribute in C#)
* DoNotAcceptEvents
* Id
* Sprite
* Text (Text Widgets only)



## WidthSizePolicy / HeightSizePolicy

??? example "Examples"
    ``` xml
    <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="590" SuggestedHeight="125" ...
    <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" ...
    <ListPanel WidthSizePolicy="CoverChildren" HeightSizePolicy="Fixed" SuggestedHeight="40" ...
    ```

- Fixed - fixed size (width/height)
    * SuggestedWidth - sets the width in pixels
    * SuggestedHeight - sets the height in pixels
- StretchToParent - the size (width/height) of the widget will be adjusted to match the size of its parent container. It means the widget will stretch horizontally or vertically to fill the available space provided by the parent. This option is useful when you want a widget to occupy the entire width of its parent container.
- CoverChildren - the size (width/height) of the widget will be determined by the size of its children. In this case, the widget will adjust its width to cover the entire area occupied by its children elements. This option is useful when you want a widget to automatically adjust its width based on the size and position of its children elements.


## HorizontalAlignment / VerticalAlignment

??? example "Examples"
    ``` xml
    <Widget VerticalAlignment="Top" HorizontalAlignment="Left" ...
    <HintWidget HorizontalAlignment="Center" ...
    <RichTextWidget HorizontalAlignment="Right" VerticalAlignment="Center" ...
    ```


These attributes allow to specify how the UI element should be positioned horizontally and vertically within its parent container.

**HorizontalAlignment**: Specifies the horizontal alignment of the UI element within its parent container. It can have the following values:

- Center: The UI element is horizontally centered within its parent container.
- Left: The UI element is aligned to the left edge of its parent container.
- Right: The UI element is aligned to the right edge of its parent container.

**VerticalAlignment**: Specifies the vertical alignment of the UI element within its parent container. It can have the following values:

- Center: The UI element is vertically centered within its parent container.
- Top: The UI element is aligned to the top edge of its parent container.
- Bottom: The UI element is aligned to the bottom edge of its parent container.

``` xml
<Widget HorizontalAlignment="Center" VerticalAlignment="Top">
    <!-- Child elements or content here -->
</Widget>
```

In this example, the Widget element will be horizontally centered within its parent container and aligned to the top edge of the container.


## MarginLeft / MarginRight / MarginTop / MarginBottom

??? example "Examples"
    ``` xml
    <Standard.PopupCloseButton MarginTop="840" ...
    <Widget MarginLeft="70" MarginTop="50" ...
    <ListPanel MarginLeft="260" MarginRight="70" MarginTop="100" ...
    ```

These attributes are used to define the margins or spacing around a UI element within its parent container. These attributes allow you to control the placement and positioning of the UI element within its layout.

- MarginLeft: Specifies the left margin or spacing of the UI element from its parent container's left edge.
- MarginRight: Specifies the right margin
- MarginTop: Specifies the top margin
- MarginBottom: Specifies the bottom margin 

These attributes accept numeric values that represent the distance in pixels. You can set them to positive values to create space between the UI element and its parent container's edges, or set them to zero if you want the UI element to touch the edges. Negative values can also be used to create overlapping effects.

``` xml
<Widget MarginLeft="10" MarginTop="20">
```

In this example, the Widget element will be positioned with a left margin of 10 pixels and a top margin of 20 pixels within its parent container. The MarginRight and MarginBottom attributes are not specified, so the widget will not have any spacing from the right and bottom edges of its parent.



## PositionXOffset / PositionYOffset

??? example "Examples"
    ``` xml
    <RichTextWidget ... PositionXOffset="-48" PositionYOffset="-44" ...
    ```

The PositionXOffset and PositionYOffset attributes are used to adjust the position of a prefab element relative to its parent or reference point.

* PositionXOffset: This attribute specifies the horizontal offset of the prefab element from its parent or reference point. A positive value moves the element to the right, while a negative value moves it to the left.

* PositionYOffset: This attribute determines the vertical offset of the prefab element from its parent or reference point. A positive value moves the element downwards, while a negative value moves it upwards.

By adjusting these offsets, you can fine-tune the placement of elements within the user interface (UI). These attributes are particularly useful when you want to align or position elements precisely according to your design or gameplay requirements.

``` xml
<ButtonWidget PositionXOffset="10" PositionYOffset="-5">
```
In this example, the button element will be shifted 10 units to the right and 5 units upwards from its parent or reference point.

!!! warning "X and Y here are not measured in the screen pixels. This is something else. On my screen with resolution 3440x1440 X and Y for this measurement system is ~2200x1000. Use 'scaled' versions of the attributed to operate in real screen pixels."


## ScaledPositionXOffset / ScaledPositionYOffset

Same as PositionXOffset / PositionYOffset but measured in real screen pixels. Can be used with MBWindowManager.WorldToScreen.


## IsVisible / IsHidden

??? example "Examples"

    ``` xml
    <ListPanel IsVisible="@IsVisible" ...
    <Widget IsHidden="true" ...
    ```

The attributes IsVisible and IsHidden are used to control the visibility of UI elements.

IsVisible: This attribute determines whether an element is visible on the screen. When set to true, the element will be displayed, and when set to false, the element will be hidden. It affects the visibility of the element itself as well as any child elements it may contain.

IsHidden: This attribute also controls the visibility of an element, similar to IsVisible. However, it is usually used in conjunction with conditions or triggers. The IsHidden attribute allows you to dynamically control the visibility of an element based on certain conditions or events. It can be bound to a script or other mechanisms to determine when the element should be hidden.


## Notes

!!! danger "Using same Attribute twice in the same Widget will crash the game"

