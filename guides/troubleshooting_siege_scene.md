# Troubleshooting crashing Siege Scene

I got a siege scene that was working on level 1 but crashing on level 2/3.

HINT: Before troubleshooting, I name the map in a way that it's always first because I will need to test a lot - this saves a lot of time avoiding clicking and searching for the map:

![](/pics/2402271318.png)

Butterlib shows:

![](/pics/2402271313.png)

Log suggests the same:

![](/pics/2402271325.png)

Ok, what is a deployment point? Google search leads to the [official documentation](https://moddocs.bannerlord.com/authoring-mission-scenes/sieges/#deployment-points):

*All siege machines are placed under deployment points. These points later in the mission become the selectable positions, in which players can place their siege equipment, like ballistas, battering ram or the siege towers.*

[Attaching the dnSpy](/resources/dnspy/#how-to-attach-dnspy-to-bannerlord-and-catch-the-exception), shows:

![](/pics/2402271322.png)

dnSpy shows that this deployment point is looking for weapons but has no weapons assigned:

![](/pics/2402271327.png)

Ok, how weapons are assigned to the deployment point? Official documentation answers that:

*The siege machines can be connected to these deployment points, by either placing them in their radius, or by tags.*

dnSpy tells which entity is causing the problem:

![](/pics/2402271330.png)

??? info "Example with another entity"
    ![](/pics/2402281753.png)

In the scene, there are 7 such entities with the same name:

![](/pics/2402271332.png)

It's possible to find out the problematic entity by it's coordinates:

![](/pics/2402271335.png)

Clicking through the entities together with the info on how weapons are assigned to the deployment point, I have noticed that its Radius value is quite different from the other entities:

![](/pics/siege_scene_crash_trb.gif)

I changed it to 15 and tested the scene again.

The scene stopped to crash. That means this was the cause of the problem.
