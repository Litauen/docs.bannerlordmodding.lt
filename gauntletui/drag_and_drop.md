# Drag and Drop

Widgets can be moved around with drag and drop

## Drag
* AcceptDrag - Set to true to make element draggable
* DragWidget - Optional, Id of element to use for display when draggin instead of the full widget.
  * The DragWidget will default to IsVisible="false", and will set it to false after drag end even if set to true in code
  * Should use fixed width and height
* HideOnDrag - Optional, whether to still display the base widget when dragging. Set to true by default, and only matters when DragWidget is provided
* Command.DragBegin - Optional, parameterless method to call when drag begins
* Command.DragEnd - Optional, parameterless method to call when drag ends

??? example "Examples"
    ``` xml
    <Widget ...  AcceptDrag="true" DragWidget="DragImage" HideOnDrag="false">
      <Children>
        <!-- Background frame -->
        <ButtonWidget WidthSizePolicy="StretchToParent" >
        <!-- Drag image -->
        <ImageIdentifierWidget Id="DragImage" DataSource="{ImageIdentifier}" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!Recruitment.Troop.Panel.Width" SuggestedHeight="!Recruitment.Troop.Panel.Height" />
        <!--  -->
        <ImageIdentifierWidget DataSource="{ImageIdentifier}" ImageTypeCode="@ImageTypeCode" WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" .../>
      </Children>
    </Widget>
    ```

## Drop
* AcceptDrop - Set to true to allow droping the draggable items onto this widget
* CommandParameter.Drop - Optional, additional string parameter to pass to the drop method
* Command.Drop - Method to call on drop, with arguments of these types:
  * \<ViewModel\> - Some specific type of your dragged ViewModel or Widget
  * int - Index of position to insert if the AcceptDrop widget is list.
    * Also, list with drops has automatic visual behavior of parting items to show insertion point
    * Otherwise for not lists the index will be -1
  * string - Only if Drop parameter was passed
  
  The order of arguments must be as above, and if the Drop parameter was used in XML, it must be also included in the method, or the game will crash. Likewise, the game will crash if the VM argument type does not match

??? example "Examples"
``` xml
<Widget AcceptDrop="true" Command.Drop="ExecuteTransferWithParameters">
  <Children>
  ...
  </Children>
</Widget>

public void ExecuteTransferWithParameters(SpellSlotVM draggedSpellVM, int index)
{
    onDrop(draggedSpellVM, this);
}
```
    
