# Textures

# Resize the texture on the object

!!! quote "[Byako](https://discord.com/channels/411286129317249035/761302555308720148/1180865670410403950): you can if the texture scaler flag is activated on the material, then you can scale it with x/y on vector 2 arguments"

![](/pics/2403161105.png)

Then in your game entity you can customize as needed:

![](/pics/2403161106.jpg)

You can also use z/w to displace the material.

!!! question "I get "unable to save material" if i try to set anything diffrent on it and "save""
    If it's a native material, you need to override it in the resource browser. Though in that case you'll need to publish the assets files along your scene.<br>
    ![](/pics/2403161108.png)