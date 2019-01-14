---
layout: post
title: "Game Engineering 2 - Assignment05"
date: 2018-01-01
excerpt: "This is the writeup for assignment05, EAE 6320 'Game Engineering 2'"
tags: [EAE6320, Game Engine, Assignment]
comments: false
---

<div markdown="0"><a href="https://drive.google.com/open?id=1MySPtaJOBkf4QgIqJpcHTWmWFo1U98G8" class="btn btn-info">Download</a></div>

## Content

In this assignment, we need to create the representation of camera and game object, and the way to submit them to graphics and move objects using keyboard. In my project, I created a base class named `cGameObject`, and two derived class `cCamera` and `cRenderableObject`.

### structure

<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/1.png"><img src="../img/blog/GameEngineering2/Assignment5/1.png"></a>
</figure>

---

### cGameObject
cGameObject has two member variables: sRigidBodyState and transform matrix. sRigidBodyState stores the physical information for Game to use, matrix stores the transform for graphics. The class has two virtual functions for now: Update and Submit, which updates sRigidBodyState and submit mMatrix and allows derived class to define its own implementation. This is used for game objects which only need a sRigidBodyState and transformation, such as trigger.

<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/2.png"><img src="../img/blog/GameEngineering2/Assignment5/2.png"></a>
</figure>

---

### cCamera

cCamera has two functions to calculate matrix for rendering, and overrides the submit function to submit its own data (two matrices) to graphics.

<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/3.png"><img src="../img/blog/GameEngineering2/Assignment5/3.png"></a>
</figure>

---

### cRenderableObject

cRenderableObject provides another function to set its mesh and effect besides its constructor. Every time a new mesh or effect is set, it increments the reference count of new mesh/effect to keep the mesh/effect valid, and decrements the reference count of old mesh/effect to make the mesh/effect can be released. When object is released, the reference count is decremented. (Copy constructor, copy assignment, move constructor, move assignment should be deleted here in order to keep the correct reference count).

<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/4.png"><img src="../img/blog/GameEngineering2/Assignment5/4.png"></a>
</figure>

---

### Submit game object

This takes a pointer of cGameObject and the time for predict the position of this game object. Inside the function, a simple call to ipGameObject->Submit is enough. Each cGameObject class could has its own submit function.

<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/5.png"><img src="../img/blog/GameEngineering2/Assignment5/5.png"></a>
</figure>

---

### Size of data for each draw call

In order to draw a mesh, graphics need a pointer to a mesh, a pointer to an effect, a matrix. The total size is 72 bytes (4+4+64) on x84, 80 bytes (8+8+64) on x64.

---

### Necessity of extrapolation

`UpdateSimulationBasedOnTime` is called based on the frame rate we set in our game. If the frame rate is 15, the function will only be called 15 times per second. This is where we update our game object in game loop. But SubmitDataToBeRendered and RenderFrame on rendering thread are called as soon as possible, which is more frequently than `UpdateSimulationBasedOnTime`. If we don’t use extrapolation, the moving object will move only when `UpdateSimulationBasedOnTime` is called, and stay at the same position when the sequent several `SubmitDataToBeRendered` are called. It could case “jerky” movement. That’s why we need to predict the position an object would be at when it is submitting to graphics.

---

### binding 2 / register b2

In sContantBuffer Bind function, it needs a binding point to bind the correct buffer. We pass the m_type to the function, and m_type is enum ConstantBufferTypes, which PerFrame = 0, PerMaterial = 1, PerDrawCall = 2. So binding 2 is binding to constant buffer per draw call, binding 0 is binding to constant buffer per frame.

<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/6.png"><img src="../img/blog/GameEngineering2/Assignment5/6.png"></a>
</figure>

---

## Controls
Press <kbd>W</kbd><kbd>A</kbd><kbd>S</kbd><kbd>D</kbd> to move the rectangle. Hold Control and press<kbd>W</kbd><kbd>A</kbd><kbd>S</kbd><kbd>D</kbd> to move the triangle. Press <kbd>Up</kbd><kbd>Down</kbd><kbd>Left</kbd><kbd>Right</kbd> Arrow to move camera.

## Screenshots

Default position
<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/7.png"><img src="../img/blog/GameEngineering2/Assignment5/7.png"></a>
</figure>

Move the triangle behind left
<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/8.png"><img src="../img/blog/GameEngineering2/Assignment5/8.png"></a>
</figure>

Move the rectangle downn
<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/9.png"><img src="../img/blog/GameEngineering2/Assignment5/9.png"></a>
</figure>

Change the rectangle mesh to triangle mesh
<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/10.png"><img src="../img/blog/GameEngineering2/Assignment5/10.png"></a>
</figure>

Move camera forward
<figure>
	<a href="../img/blog/GameEngineering2/Assignment5/11.png"><img src="../img/blog/GameEngineering2/Assignment5/11.png"></a>
</figure>

## Discussion

The system of game object could be placed in another project because it’s not directly related to Graphics project. All it needs are meshes, effects, transformation which. Game object could submit the data to graphics and store some other staff related to gameplay or some other systems.