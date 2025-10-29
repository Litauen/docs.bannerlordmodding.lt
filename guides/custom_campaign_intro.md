# Custom Campaign Intro

!!! info "Guide by Mermen12"

If you're using SandboxGameManager, I changed it with this transpiler patch:

```cs
[HarmonyPatch(typeof(SandBoxGameManager))] 
HarmonyPatch("OnLoadFinished")]
public static class ReplaceCampaignIntroVideoPatch
{
    private static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
    {
        List<CodeInstruction> list = new List<CodeInstruction>(instructions);
        for (int i = 0; i < list.Count; i++)
        {
            if (list[i].opcode == OpCodes.Ldstr)
            {
                string text = list[i].operand as string;
                List<Label> labels = list[i].labels;
                if (text == "SandBox")
                {
                    list[i] = new CodeInstruction(OpCodes.Ldstr, "YourMod");
                    list[i].labels = labels;
                }
                else if (text == "campaign_intro")
                {
                    list[i] = new CodeInstruction(OpCodes.Ldstr, "yourvideo");
                    list[i].labels = labels;
                }
                else if (text == "campaign_intro.ivf")
                {
                    list[i] = new CodeInstruction(OpCodes.Ldstr, "yourvideo.ivf");
                    list[i].labels = labels;
                }
                else if (text == "campaign_intro.ogg")
                {
                    list[i] = new CodeInstruction(OpCodes.Ldstr, "yourvideo.ogg");
                    list[i].labels = labels;
                }
            }
        }
        return list.AsEnumerable();
    }
}

```

Place the .ivf and .ogg files in `YourMod/Videos/CampaignIntro`.

It should work the same with `StoryModeGameManager` (for 1.2.12)

Use `ffmpeg` to convert your mp4 to .ivf:

    ffmpeg -i yourvideo.mp4 -c:v libvpx -crf 4 -b:v 2M yourvideo.ivf

Then to convert your mp3 file to .ogg:

    ffmpeg -i yourvideo.mp3 yourvideo.ogg

Make sure that the file name is different from `campaign_intro.ivf` and `campaign_intro.ogg`. Otherwise it won''t work.