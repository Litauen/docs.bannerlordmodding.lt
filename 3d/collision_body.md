# Collision Body

A collision body in 3D cloth simulation is a rigid object that the cloth can interact with during the simulation.

What it does:

* Prevents cloth from passing through solid objects (like characters, furniture, walls)
* Provides realistic interaction when cloth hits or drapes over objects
* Maintains physical boundaries in the simulation space

How it works:

* The simulation detects when cloth vertices get too close to the collision body
* Applies forces to push cloth away from or around the object
* Calculates contact points and adjusts cloth position/velocity accordingly

Common examples:

* Character body - cloth doesn't clip through the person wearing it
* Ground/floor - cloth falls and settles on surfaces
* Furniture - cloth drapes over chairs, tables, etc.
* Walls - cloth bounces off or slides along barriers

Types:

* Simple shapes: Spheres, boxes, capsules (fast collision detection)
* Complex meshes: Detailed character bodies, furniture (more accurate but slower)

Essentially, collision bodies are the "solid things" that make cloth behave realistically instead of just falling through everything in the virtual world.


## Custom Collision Body

!!! warning "One collision body can be used for many clothes. So if you need different collision body - create a new one and modify it. Do not touch present collision bodies!"

Let's create a new collision body.

In the Cloth Editor window:

![](/pics/2506191851a.png)

![](/pics/2506191851b.png)

Give it a name:

![](/pics/2506191851c.png)

Select your module here:

![](/pics/2506191851d.png)

Select proper skeleton:

![](/pics/2506191851e.png)

As newly created collision body is empty, it is easier to copy capsules from existing collision body and adjust to your liking:

![](/pics/2506191851f.png)

![](/pics/2506191851g.png)

Copy all of them:

![](/pics/2506191851h.png)

For some reason (TW...) bones are not assigned after the copy.

Check bones in original, assign same bones to your collision body capsules:

![](/pics/2506191851i.png)

![](/pics/2506191851j.png)

Test in Cloth Editor:

![](/pics/2506191851k.png)

Choose your collision body:

![](/pics/2506191851l.png)

Red color shows what you have selected:

![](/pics/2506191851m.png)

Adjust position.

Press r to adjust rotation.

!!! note "When making changes - Save and changes are applied to the cloth/model instantly, no need to regenerate cloth physics."

!!! warning "SAME WARNING: Collision body position (when moving capsules aroud) and rotation is applied to all the models that use this collision body. Moving/Rotating on one model, applies changes to all other related models. If you need a different location/size/capsule -> create a new collision body and use it separately."