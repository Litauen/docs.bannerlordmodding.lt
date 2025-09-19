# Prefabs

    /GUI/Prefabs/.xml

Defines various GUI elements, such as windows, screens, buttons, etc.

In the game "Movies" are constructed from Prefabs and ViewModel datasource.

Prefabs - groups of [Widgets](/gauntletui/widgets)

## Structure

``` xml
<Prefab>
    <Constants>
        <!-- Insert Summary Here -->
    </Constants>
    <Variables>
        <!-- Insert Summary Here -->
    </Variables>
    <VisualDefinitions>
        <!-- Insert Summary Here -->
    </VisualDefinitions>
    <Window>
        <!-- Insert Summary Here -->
    </Window>
</Prefab>
```

## Constants

Defines constant values that can be used throughout the prefab.

The values can be referenced using the `!` symbol (e.g. `Text="!ConstantName"`).

There are several types of constants:

### Constant Value
Assigns a constant value.

``` xml
<Constant Name="SidePanel.Width" Value="90" />
<Constant Name="SidePanel.NegativeWidth" MultiplyResult="-1" Value="!SidePanel.Width" />
```

### Boolean Check
If `BooleanCheck` condition is true, assign the `OnTrue` value, else assign the `OnFalse` value.

``` xml
<Constant Name="DropdownCenterBrush" BooleanCheck="*IsFlatDesign" OnFalse="SPOptions.Dropdown.Center" OnTrue="MPLobby.CustomServer.CreateGamePanel.DropdownButton" />
```

### Brush Sprite Size
Retrieves the content width or height of a brush sprite based on the BrushValueType parameter.

Bannerlord uses [9-slice](https://en.wikipedia.org/wiki/9-slice_scaling) approach, so the actual content size is smaller than the sprite size.

??? example "Calculation:"

    Calculation:
    - Horizontal: `Sprite.Width - ExtendLeft - ExtendRight`
    - Vertical: `Sprite.Height - ExtendTop - ExtendBottom`

    Where `Extend*` values are defined in the brush layer.


``` xml
<Constant Name="PartyToggle.Width" BrushLayer="Default" BrushName="LeftPanel.Header" BrushValueType="Width" />
<Constant Name="PartyToggle.Height" BrushLayer="Default" BrushName="LeftPanel.Header" BrushValueType="Height" />
```

### Sprite Size
Assigns the width or height of a sprite.

``` xml
<Constant Name="BarterToggle.Shadow.Width" SpriteName="PartyScreen\button_collapser_shadow" SpriteValueType="Width" />
<Constant Name="BarterToggle.Shadow.Height" SpriteName="PartyScreen\button_collapser_shadow" SpriteValueType="Height" />
```

### Common Attributes
These attributes can be used in any constant definition:

- `Additive`: Adds the given value to the result (only for numeric values)
- `MultiplyResult`: Multiplies the result by the given value (only for numeric values)
- `Prefix`: Prepends the given value to the result
- `Suffix`: Appends the given value to the result


## Variables
This section is not used in the current codebase and is likely deprecated.

## VisualDefinitions
This section is used for animating widget properties.

??? example "Example:"
    Slides the panel in from the left when it appears.

    ``` xml
    <VisualDefinitions>
        <VisualDefinition Name="LeftPanel" EaseIn="true" TransitionDuration="0.4">
            <VisualState PositionXOffset="0" State="Default" />
        </VisualDefinition>
    </VisualDefinitions>
    <!-- ... -->
    <Window>
        <Widget Type="Panel" VisualDefinition="LeftPanel" PositionXOffset="-630">
            <!-- ... -->
        </Widget>
    </Window>
    ```

### VisualDefinition Attributes:
- `Name`: The name of the visual definition (used to reference it in widgets).
- `TransitionDuration`: The duration of transitions between states (in seconds).
- `EaseIn`: `"true"` or `"false"`. Whether to use ease-in interpolation for transitions.
- `DelayOnBegin`: Delay before starting the transition (in seconds).

### VisualState Attributes:
- `State`: The name of the state (e.g., `"Active"` or `"Inactive"`). Set to `"Default"` for the default state.
  - Can be changed programmatically using `Widget.SetState(string state)` method.
- **Widget attributes:** Any widget attributes that can be animated, such as `PositionXOffset`
