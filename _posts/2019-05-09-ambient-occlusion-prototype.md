---
layout: post
title:  "Ambient Occlusion Prototype"
date:   2019-05-09
---

As explained in the previous post, computing light bounces is an expensive operation.
Fortunately we can achieve a closely accurate result using the ambient occlusion technique.

The principle is rather simple, it comes from the observation that closed spaces such as concave corners tend to be darker than convex areas.
Moreover this is applies indepently from the position of the light source (on a relative scale).
This assumption is quite fair in the sense that open areas are likely to be more exposed to the light.

The "openness" of an area is determined by integrating the bounded rays over the hemisphere at that point, divided by the total area of the hemisphere.
The hemisphere is oriented outwards in respect to the normal vector.
The radius of the hemisphere is an arbitrary value that corresponds to the local area that must be considered.
Larger values create more realistic renderings but are possibly also more expensive.

{% figure caption:"The point of interest is represented in magenta. The hemisphere orientation is determined by the normal vector of the surface (n). The sampled rays are in blue and red depending on wether they intersected in a surface or not. The green area is the sought area" %}
![](/img/ambient_occlusion_hemisphere.svg){: .image-medium }
{% endfigure %}

Now the question arises on how to implement such a procedure.
Since it is an improvement on the regular ambiant lighting system we updated this function such that instead of returning a constant value it would be affected by the position of the intersection.
The integral can be approximated using a stochastic sampling process: we will cast random rays and sum their results.
The trick used to generate a random vector in a sphere is to first generate a random vector in a cube (trivial), and discard it if its norm is greater than one.
Finally if the scalar product between the vector and the normal is negative that means it is not in the hemisphere, so the opposite vector is chosen instead.

{% figure caption:"Scene rendered without colors and only ambient occlusion; radius was set to 1 distance unit and sampling to 300 rays per pixel" %}
![](/img/ambient_occlusion_alone.bmp){: .image-large }
{% endfigure %}

{% figure caption:"The same scene with the previous effects" %}
![](/img/ambient_occlusion_all.bmp){: .image-large }
{% endfigure %}

The results are quite encouraging, even though the rendering time has drastically increased (the previous images took half an hour to complete).
As we are working on implementing an efficient queriable structure for the scene, we are confident that this issue will be solved or at least greatly reduced when we merge the two.
In fact it would be fairly easy to first filter out all the triangles that are out range before doing the sampling process.
