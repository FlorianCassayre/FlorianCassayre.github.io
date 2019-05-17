---
layout: post
title:  "Octree Querying"
date:   2019-05-15
---

In this second post we will describe the actual usage of the tree.
This routine is stored in black-box function that efficiently finds the closest intersection between a ray and a triangle.

Firstly we need to define the ray-cube intersection.
Each AABB has 6 faces, we have to find the face with the closest intersection (if it exists).
As an example, let's take one of the two faces aligned with the $$z$$ axis on $$z_0$$:

$$P + t \cdot \vec{r} = \begin{pmatrix} x \\ y \\ z_0 \end{pmatrix}, t \geq 0$$

Assuming the ray is not parallel to the plane, we have a single solution:

$$t_0 = \frac{z_0 - z_p}{z_r}$$

$$\begin{cases} x = x_p + t_0 \cdot x_r \\ y = y_p + t_0 \cdot y_r \end{cases}$$

First we need to check that the point in indeed on the face $$x_{min} \leq x \leq x_{max}$$ and $$y_{min} \leq y \leq y_{max}$$.
These properties should only hold for either 0 or 2 faces at the same time (because the cube is a convex set).
In the first case we can safely return an empty result; while in the second case the smallest value $$t_0$$ of the two should be returned.

![](/img/quadtree_querying.svg){: .image-large }

{: .caption }
Each square is recursively tested in-order, sorted by the distance from the closest intersection from the source of the ray.
When a leaf is reached all the line segments (the triangles in our case) are tested against the ray.
Once an intersection is found, the result is guaranted to be the closest thus the function breaks.

Now that we have our ray-cube intersection, we can build the octree collision detection function.

If the cube has children, we filter and sort them by their value of $$t_0$$ defined previously.
Then we recursively call the same function on these cubes ordered in ascending order.
If a cube returns an intersection, the iteration stops and the result is propagated back to the initial caller.

Otherwise if the cube doesn't have any children (base case), we perform a ray-triangle intersection test with all the triangles of that node.
