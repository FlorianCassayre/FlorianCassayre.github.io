---
layout: post
title:  "Procedural Textures"
date:   2019-05-20
---

The checkerboard pattern started to become too basic for our fancy model.
We wanted something less regular; that's when the texture enters.

And what's more fun than letting the computer create the textures for you?
In fact one of our idea was to have the model stand on a wooden table, therefore we needed some sort of wood-ish texture.
Well it turns out it's really not difficult to generate wood using procedural noise.

Fortunately for us the glm library supports [simplex noise](https://en.wikipedia.org/wiki/Simplex_noise).
Here are the operations we found looked good:

1. Choose one of the two coordinates and scale it by an arbitrary factor.
2. Set the other coordinate to a constant value and sample a noise value $$n_{displacement}$$ at that point.
3. Set the other coordinate back to its original value plus $$n_{displacement}$$, and sample two other noise values $$n_1, n_2$$ at a various scaled displacement.
4. Apply a smooth periodic function such as $$sin$$ to $$n_1, n_2$$ with different periods, add them together and normalize the result $$n$$.
5. Finally blend two colors $$n \cdot c_1 + (1 - n) \cdot c_2$$.

![](/img/wood_texture.bmp){: .image-large }

{: .caption }
A sample of the final texture.
You will see later that with lower contrasts it's hard to tell that it is not synthetic.

This function can thereafter be used a texture that's computed _on the fly_ (it's not particularly expensive so we can allow it).
Here we decided to stick to the checkerboard by alternating with two different types of woods.
A small trick is to use a different seed for each wood type to mark the transition.

![](/img/dragon_wood.bmp){: .image-large }

{: .caption }
The final result, our best image so far.
