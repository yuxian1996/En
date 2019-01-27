---
layout: post
title:  "Realtime Rendering - Local-to-projected Transform"
date:   2019-01-26
excerpt: "Pass a local-to-projected transform to our shaders to get rid of the multiplication of three transforms "
tag: [EAE6900, Assignment, Realtime Rendering]
comments: false
---
 
### Use local-to-projected transform

We can get a local-to-projected transform in our C++ code and pass it to shader so that it doesn’t have to calculate this transform in every shader, which might cost more time. The order of concatenate is important because normally the matrix multiplication doesn’t meet commutative law. The order of transform we set in our engine is from right to left: `camera-to-projected * world-to-camera * local-to-world`. Including intermediate transform such as (world-to-camera * local-to-world) transform is not a good idea for two reasons:

1.	We don’t often use those transforms

2.	When we use it, it’s simple to get those transforms from multiplication of other transforms since we already pass those three transforms to shaders

It’s waste of memory if we pass those transform to the shaders which don’t even use them.


### Number of Instructions

The number of instructions of our vertex shader before (use three matrices in vertex shader) is **16**. The number of instructions of the vertex shader using one local-to-projected transform is **9**. It’s obvious that it’s good to use single concatenated transform other than three transforms from the number of instructions 
