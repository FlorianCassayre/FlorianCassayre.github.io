---
layout: post
title:  "Project Specifications"
date:   2019-05-08
---

The main focus of our project was to improve the raytracer from the lab. We will focus on the following main tracks:

- **Scene representation**

  Currently the rendering is very slow, the main reason being that the intersection pipeline is not optimal.
  Each time such an intersection is computed, all the triangles of the scene are checked.
  This can greatly be improved by segmenting the space and cutting expensive operations by only checking triangles that are possibly intersecting.
  Therefore we are planning to implement an **octree** of triangles, aswell as some other optimization strategies.

- **Better shading and lighting**

  The current light simulation is rather basic and only allows the existence of binary shadows.
  In reality the movement of light is more complex than a direct exposition, in fact the photos generally bounce in another direction when hitting a surface.
  Thus to create a realistic simulation it's not sufficient to only consider the direct light.
  In practice simulating bounces is an exponentially complex problem.
  In this project we will try different techniques that approximate this effect such as **ambiant occlusion** and **light tracing**.
  We might aswell consider simulating other phenomena like reflexion and refraction on different materials, distance blur, fog...

In addition to that, we plan to include some of the following minor features:

- Loading scenes from a file, in some possibly custom format
- Antialiasing
- Allow triangles to hold a bitmap texture instead of a single color
- Possibility of rendering a sequences of images with a moving camera

