---
layout: post
title:  "Realtime Rendering - Local-to-projected Transform"
date:   2019-01-26
excerpt: "Pass a local-to-projected transform to our shaders to get rid of the multiplication of three transforms "
tag: [EAE6900, Assignment, Realtime Rendering]
comments: false
---
 
### Use local-to-projected transform

There are two ways to concatenate the local-to-projected transform:

1. (camera-to-projected * world-to-camera) * local-to-world

2. camera-to-projected * (world-to-camera * local-to-world)

camera-to-projected * world-to-camera is constant in one frame, if we concetenate them first, we only have to do 1 + n calculation. world-to-camera * local-to-world is different in every draw call, if we concetenate them first, we have to do n * n calculation.

camera-to-projected is an affine matrix. if we concatenate it with another matrix, the result is always something like this:
    \\[ \begin{bmatrix} a11 & a12 & a13 & 0 \\\
    a21 & a22 & a23 & 0 \\\
    a31 & a32 & a33 & 0 \\\ 
    a41 & a42 & a43 & 1 
    \end{bmatrix} \\]
Therefore we can only calculate the first three columns and simply set the last column as [0, 0, 0, 1]. If concatenate order is the first one, we can save more time by replace 0 and 1 to the original matrix multiplication. If concatenate order is the second one, we only save once from matrix4x4 * matrix4x4 to affine matrix4x4 * matrix4x4. 

In conclusion, concetenating the first two matrices first is better.
 
We can get a local-to-projected transform in our C++ code and pass it to shader so that it doesn’t have to calculate this transform in every shader, which might cost more time. Including intermediate transform such as (world-to-camera * local-to-world) transform is not a good idea for two reasons:

1.	We don’t often use those transforms

2.	When we use it, it’s simple to get those transforms from multiplication of other transforms since we already pass those three transforms to shaders

It’s waste of memory if we pass those transform to the shaders which don’t even use them.


### Number of Instructions

The number of instructions of our vertex shader before (use three matrices in vertex shader) is **16**. The number of instructions of the vertex shader using one local-to-projected transform is **9**. It’s obvious that it’s good to use single concatenated transform other than three transforms from the number of instructions 
