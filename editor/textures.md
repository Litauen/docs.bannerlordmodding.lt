# Textures

* [Official documentation](https://moddocs.bannerlord.com/asset-management/asset-types/textures/)

## Names

`_d`, `_n`, and `_s` are a convention, not a requirement. A material slot will take any texture you assign.

Use the suffixes anyway. On import, the Resource Browser reads the ending and sets the compression and the map type:

| File | Map | Slot |
| --- | --- | --- |
| `name_d` | Diffuse / base color (sRGB) | `DiffuseMap` |
| `name_n` | Normal map | `NormalMap` |
| `name_s` | Specular / gloss / metallic | `SpecularMap` |

`pbr_shading` and `pbr_metallic` both sample those three slots. `pbr_metallic` is the metallic workflow. If the three files share one name, choosing the `_d` in `DiffuseMap` fills `NormalMap` and `SpecularMap` for you. See [Assign Textures](/3d/material/#assign-textures).

On the `_s` map, red is metallic, green is smoothness, and blue is ambient occlusion. See [Import Textures](/3d/material/#import-textures).

## Put textures on a model

1. Import the `_d`, `_n`, and `_s` files into the module's Assets in the [Resource Browser](/editor/resource_browser/).
2. Open the material.
3. Set the shader to `pbr_shading`, or to `pbr_metallic` when the `_s` map is metalness.
4. Assign `_d` to `DiffuseMap`, `_n` to `NormalMap`, and `_s` to `SpecularMap`. Names that do not match still work if you pick each slot yourself.
5. Under **Material Shader Flags**, turn on `use_specular`. Under **Vertex Layout**, turn on Bumpmap, and Skinning when the mesh is skinned (armor, clothing). The [Set Settings](/3d/material/#set-settings) screenshot lists both checkboxes.
6. Save the material and the mesh.

# Resize the texture on the object

!!! quote "[Byako](https://discord.com/channels/411286129317249035/761302555308720148/1180865670410403950): you can if the texture scaler flag is activated on the material, then you can scale it with x/y on vector 2 arguments"

![](/pics/2403161105.png)

Then in your game entity you can customize as needed:

![](/pics/2403161106.jpg)

You can also use z/w to displace the material.

!!! question "I get "unable to save material" if i try to set anything diffrent on it and "save""
    If it's a native material, you need to override it in the resource browser. Though in that case you'll need to publish the assets files along your scene.<br>
    ![](/pics/2403161108.png)