---
layout: post
title:  "Octree Construction"
date:   2019-05-13
---

All the effects we previously presented are very slow with our current intersection checking routine.
In order to be able to combine them together, we would need an astronomical amount of time to generate a single frame.
Thus it was absolutely vital to improve it.

To do that we need to segment the space (a list of triangles) into smaller fragments. Several methods exist: bounding boxes, bounding spheres, etc.
We decided to work with axis aligned bounding boxes (will be further noted AABB) using an octree.
This particular orientation of the boxes is very important because it makes the computations much easier for the machine (and for us!).

The octree is a partioning system that wraps the scene inside a cube, this cube is then split into 8 smaller cubes of equal sizes that are also AABB, these smaller cubes are split again in 8 even smaller cubes, and so on until we reach an arbitrary threshold or some condition.
The idea is that the depth of the resulting tree is proportional to the logarithm of the size difference between the biggest cube and the smallest one, and a query would only require that many operations.
The code for the octree is divided into two independent steps: the **construction** and the **querying**.

In this first post we will briefly describe the construction. Note that in our case this procedure does not need to be optimal as it is only going to be run once, at the beginning.

As a starting point we need to find the smallest box that contains all the triangles.
This is done in a single pass and will serve as the base case of our recurrence.
The boxes can be represented by only two points, the minimum and maximum corners, and they contain a list of triangles and an optional list of boxes.
Upon splitting a cube, we would like to know which triangles intersect a cube.
A simple filter that we started with is to compute the bounding sphere of that triangle (which is nothing more than the circumscribed sphere) and then process the sphere-cube intersection (which is easier).
While this method is guaranteed to preserve all positives, it can also misclassify a lot of negatives.
Then, as long as there are two or more triangles and the threshold is not reached, the branching continues.
Hopefully the number of triangles in each child is reduced by a factor at each step.

