# Engine Notes

!!! warning "These are notes about the engine by SuicideParty - for the 'future brave boys' :) "

The first and most important notice TW engine has fucked up binding architecture. it looks like that arch was planned by junior C++ dev

![](/pics/2506171913a.png)

the important part

they have atleast 6 of those binding arrays inside engine, but that's still not the worst part

each of those is a part of Mission struct instance

and each of them have the constructor like:

![](/pics/2506171913b.png)

So in the end we have architecture like

Mission factory -> mission init -> At least 6 of individual array constructors with prehardcoded 2040 value

the only real place this can be patched by code injection of binary one is mission factory, then byte signature search for constructor that involves agent size nd memory patch the every constructor that fell under needed parameters

can be changed like:

![](/pics/2506171913c.png)

just for overall engine curiosity it's made as hell bad

the thing is iam not sure thats not an disassembly artifact

jake uses disassembly to pseudocode plugin

so there can hide the array size jump table or linker section

but yes it's 100% that they hold 6 damn binder arrays

the arch is fuckedup

Also use full for memory allocation understanding:

![](/pics/2506171913d.png)

jake is still learn how low level compilation and linking works, iam at other side more an vision provider in a deep arch knowledge, but lack of time

but this case I really lack of ideas for good solution, so yes, I really need any ideas especially your one because you, atleast understood what the hell iam talking about

for the dynamic patching we made runtime memory signature analyzer

for hardcoded things I've used native memory pointers and void*

but this is hell

I even can provide memory code injection tool

there are 2 solutions on my mind.

× The dynamic patcher.<br>
   at game start it's just patch new size by searching signature.

× The static linker.<br>
   Find in a native compiled file codecave at 9999 bit size and write there an value, use this file start pointer offset as new size and patch this memory offset to every constructor

both of solutions are incredibly time consuming nor error-prone

literally every step here is unsafe / not debuggable / can lead to unpredictable execution

so yeah, such a full arch forces to write down lifelong parser or signatures list

that's a shit ton of boilerplate and ppl keeps request this mod to me

it's possible in 3 "easy" steps

1) Find in disassembly EVERY of those constructors.<br>
2) Prepare an byte - patching tool<br>
3) Recompile engine .dll with those patches<br>

and literally at every single next step you would suffer of memory corrupted

and that would be useless after next update

that's the signature scanning route, as antiviral companies do, or cheat software finds needed functions at memory, but if they change one single byte at source - redo whole analysis

this seems an nightmare to do

all I can as reverse - engineer

Find right place at memory, take an hex view and remember byte - string of assembly binary code compiled + some constant things, and search whole process memory byte by byte for the match

when it's match we are at right place, but if they updated dll the byte string change and there's no match!

and thats the best we can

as soon they plan an update and DLC - I really don't want to make dumb hell work, but it's hard to explain

well, even if i leave this game modding because I do develop my own you have the @shortjake

![](/pics/2506171913e.png)

I tried to explain him all the basis he ever can need for engine mod

But you should warn future "I wanna try" boys that is computer science specific thing and requires some deep understanding

[https://github.com/keystone-engine](https://github.com/keystone-engine)

[https://github.com/gaasedelen/patching](https://github.com/gaasedelen/patching)

the 2 tools that future modders would need to mod engine

just if someone would be brave enough to try

that's a good start where they should learn how to manually manage stack and memory and codeinjections

also, to inject code in mid of engine native function

as harmony does

[https://www.codeproject.com/Articles/44326/MinHook-The-Minimalistic-x-x-API-Hooking-Libra](https://www.codeproject.com/Articles/44326/MinHook-The-Minimalistic-x-x-API-Hooking-Libra)

harmony for C

the MinHook

[https://www.nexusmods.com/mountandblade2bannerlord/mods/7724](https://www.nexusmods.com/mountandblade2bannerlord/mods/7724)

the mod that makes engine hardcoded things availble in c#

