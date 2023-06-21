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
* [Brush](/gauntletui/brushes)
* Command.Click (Command.YourKeyHere)
* DataSource (Properties with DataSourceProperty Attribute in C#)
* DoNotAcceptEvents
* Id
* Sprite
* Text (Text Widgets only)



## WidthSizePolicy / HeightSizePolicy

??? example "Examples"
    <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="590" SuggestedHeight="125" ...<br>
    <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" ...<br>
    <ListPanel WidthSizePolicy="CoverChildren" HeightSizePolicy="Fixed" SuggestedHeight="40" ...<br>

- Fixed - fixed size (width/height)
    * SuggestedWidth - sets the width in pixels
    * SuggestedHeight - sets the height in pixels
- StretchToParent - the size (width/height) of the widget will be adjusted to match the size of its parent container. It means the widget will stretch horizontally or vertically to fill the available space provided by the parent. This option is useful when you want a widget to occupy the entire width of its parent container.
- CoverChildren - the size (width/height) of the widget will be determined by the size of its children. In this case, the widget will adjust its width to cover the entire area occupied by its children elements. This option is useful when you want a widget to automatically adjust its width based on the size and position of its children elements.


## HorizontalAlignment / VerticalAlignment

??? example "Examples"
    <Widget VerticalAlignment="Top" HorizontalAlignment="Left" ...<br>
    <HintWidget HorizontalAlignment="Center" ... <br>
    <RichTextWidget HorizontalAlignment="Right" VerticalAlignment="Center" ...


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
    <Standard.PopupCloseButton MarginTop="840" ... <br>
    <Widget MarginLeft="70" MarginTop="50" ... <br>
    <ListPanel MarginLeft="260" MarginRight="70" MarginTop="100" ...

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




## Notes

!!! danger "Using Attribute twice in the same Widget will crash the game"