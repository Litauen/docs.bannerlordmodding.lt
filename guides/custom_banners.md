# Custom Banners

!!! info "For v1.3.2b+"

## Bannerlord Banner Editor

[https://bannerlord.party/banner/](https://bannerlord.party/banner/)


Some colors changed in 1.3.2b, marked with X produce light blue for some reason:

![](/pics/2511021728.png)

![](/pics/2511021729.png)


## Banner Layer Limit

In 1.3.2b banner layer limit is 32, Harmony patch to remove it:

```cs
// disable banner layer limit (32)
[HarmonyPatch(typeof(Banner), "TryGetBannerDataFromCode")]
public class TryGetBannerDataFromCodePatch
{
    [HarmonyTranspiler]
    public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
    {
        var codes = new List<CodeInstruction>(instructions);
            for (int i = 0; i < codes.Count; i++)
        {
            // Find the constant 32 used in the comparison
            if (codes[i].opcode == OpCodes.Ldc_I4_S &&
                codes[i].operand is sbyte operandValue &&
                operandValue == 32)
            {
                // Replace 32 with int.MaxValue so the condition (count > int.MaxValue) is always false
                codes[i] = new CodeInstruction(OpCodes.Ldc_I4, int.MaxValue);
                break;
            }
        }
            return codes;
    }
}
```


## Kingdom Colors

Setting

```xml
primary_banner_color="0xff000000"
secondary_banner_color="0xff000000"
```

for some kingdoms helps in some cases also.