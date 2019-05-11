---
layout: post
title:  "Soft Shadows"
date:   2019-05-10
---

Now that we have improved the ambient component of our lighting pipeline, we will now focus on the direct lighting.
Currently the direct lighting of each virtual point falls into two case:

- the point is directly exposed to the light source, in that case the radiation formula is used
- an object obstructs the light source, then this component is zero

This simple procedure produces sharp but unreaslistic shadows.
In fact in real life the shadows appear blurry especially when the distance between the occluding object and the surface is large.

The trick we used to create this effect relied once again on sampling rays from the virtual point, but this time in the direction of the light source and with a random displacement.
The ratio between the number of ray that did not reach the light source and the total rays casted corresponds to the intensity of the shadow.

Unfortunately the task of randomly (and uniformly) displacing a ray is less trivial to accomplish.
However it turns out we can use another trick that relies on the fact that $$\sin(\alpha) \approx \alpha$$ for small angles.
This property can be exploited in our case; the displacement would be a small vector bounded by a cube that gets added to the normalized vector pointing towards the light source.
Since the displacement is expected to be very small, the error would be virtually unnoticeable.

![](/img/hard_soft_shadows.bmp){: .image-huge }

{: .caption }
Hard shadows on the left figure and soft shadows on the right (100 samples per pixel)

During our experiments we encountered artifacts near the edges (the left shadow on the figure incorrectly appears brigther in the corner).
We are not quite sure yet what is causing them, we are suspecting floating point precision issues but that's only speculative.
