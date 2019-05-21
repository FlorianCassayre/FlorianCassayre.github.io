---
layout: post
title:  "Reflections"
date:   2019-05-19
---

Until now our materials reflected the light in all directions.
Polished materials don't behave like that, the light is mostly reflected in a single direction.
Today we implemented more parameters that controlled this behavior.

![](/img/reflection_refraction.svg){: .image-medium }

{: .caption }
When a ray hits a perfectly smooth surface (red), part of its energy is reflected (blue), some other part is refracted (green), and finally a little bit is absorbed (not represented).

We decided not to do refractions because we lacked time.
The reflection were fairly easy to implement.
We used a recursive function that represented a ray bouncing from triangle to triangle until it reached a maximum number of iterations or did not intersect with anything.
Given a normalized ray $$\vec{r}$$ and a normal vector $$\vec{n}$$, the reflected ray $$\vec{r}'$$ may be obtained using the following relation:

$$\vec{r}' = \vec{r} - 2 (\vec{r} \cdot \vec{n}) \vec{n}$$

We also implemented a specularity feature for shiny surfaces.
The simulated light source will appear to be reflected on the surface towards the observer.

![](/img/dragon_reflections_blur.bmp){: .image-large }

{: .caption }
As we include more features the images become more and more realistic, such as this metallic dragon.
