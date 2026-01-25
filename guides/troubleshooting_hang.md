# Troubleshooting Game Hang

Problem: game stops working when loading a battle scene on the loading screen.

## Logs

Game RGL log shows that mission started to load but then stopped.

??? info "Last log lines"
    [09:48:56.146] Update cached and formation values on each agent complete, number of selected formations: 1<br>
    [09:48:56.146] Set random decide time of agents with indices complete, number of selected formations: 1<br>
    [09:48:56.146] After set order loop complete, number of selected formations: 1<br>
    [09:48:56.146] Selected formations being cleared.

## Deadlock?

Analyzing the Wait Chain shows that there is no deadlock.

![](/pics/2410281201.png)

??? info "Analyzing the Wait Chain"
    * In the Analyze Wait Chain window, Resource Monitor displays a chain of processes and threads. Each thread may be waiting for another thread to release a resource.<br>
    * If you see a wait chain, you’ll see a tree-like structure showing which threads are blocked or waiting. This indicates a possible deadlock if threads are waiting on each other in a cycle.<br
    * Deadlock Example: If thread A is waiting on thread B, and thread B is also waiting on thread A, they’re in a deadlock.


## Threads

In Visual Studio:

Make sure `Just My Code` is turned off in debug settings:

![](/pics/2601241702.png)

![](/pics/2410281201b.png)

![](/pics/2410281201c.png)

Shows that it hangs in `PreloadHelper.WaitForMeshesToBeLoaded`


## DnSpy

Let's use Breakpoints to troubleshoot:

![](/pics/2410281201d.png)

Troubleshhoting shows that problem is with the specific shield.

## XML

In XML I have (result of fast copy-paste):

```xml
    <Item
        id="lt_teutonic_shield_anno"
        name="{=lt_teutonic_shield_anno}Anno von Sangershausen's Shield"
        body_name="lt_teutonic_shield_anno"
        shield_body_name="lt_teutonic_shield_anno"
```

And it should be like this (`bo_cap` and `bo` missing in `body_name` and `shield_body_name`):

```xml
    <Item
        id="lt_teutonic_shield_anno"
        name="{=lt_teutonic_shield_anno}Anno von Sangershausen's Shield"
        body_name="bo_cap_lt_teutonic_shield_anno"
        shield_body_name="bo_lt_teutonic_shield_anno"
```

The mistake in shield's description hanged the game because it waited indefinitely till the wrong mesh will be loaded.
