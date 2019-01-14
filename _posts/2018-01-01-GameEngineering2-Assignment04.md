---
layout: post
title: "Game Engineering 2 - Assignment04"
date: 2018-01-01
excerpt: "This is the writeup for assignment04, EAE 6320 'Game Engineering 2'"
tags: [EAE6320, Game Engine, Assignment]
comments: false
---

<div markdown="0"><a href="https://drive.google.com/open?id=1fLsndL1TFUFSmJ8UCaZ1G10V5xYgRPuB" class="btn btn-info">Download</a></div>

## Content

In this assignment, the things we’ve done before to initialize, render (bind), cleanup a mesh (effect) and change background color were moved from Graphics to Game, which made our project flexible for game engineer to use from now. Several interface were created for game engineer and game engineer (me) learned how to use the virtual function JP created before to control the game and graphics process.

### Submit Background Color

The interface to submit a background color is showed below, game engineer should pass at least three floats which indicated its red, green, blue color. The last parameter is the alpha of a color, whose default value is 1.0f, which means it’s not transparent at all. Game engineer should call this function in the function `SubmitDataToBeRendered`.

~~~ c++
void SubmitBackgroundColor(const float r, const float g, const float b, const float a = 1.0f);
~~~

### Submit mesh using effect

The interface to submit a mesh using an effect is showed below. Game engineer should pass a pointer of cMesh and a pointer of cEffect to this function. Graphics will have the reference to these two instances and render them in the order being submitted. Game engineer should call this function in the function `SubmitDataToBeRendered`.
~~~ c++
void SubmitMeshWithEffect(cMesh* ipMesh, cEffect* ipEffect);
~~~

### Reason to cache data
Game and Graphics run in different thread, it means they run simultaneously. If we draw meshes immediately, it may happen that game loop waits for graphics finishing rendering or graphics waits for game sending data. It’s a waste of time apparently. If we cache our data into two buffers, game can be running and sending data while graphics is rendering the last buffer it sent.

### Class Size

The first time I put the reference count in the top of my classes, it turned out the size of mesh is 20 bytes on OpenGL, 40bytes on Direct3D, the size of effect is 20 bytes on OpenGL, 48 bytes on Direct3D. Because the order of data stored in a class could impact the alignment of each data, it’s better to store larger in the top and stores smaller data in the bottom.

---

The size of my mesh on OpenGL after reordered is 16 bytes, the order is:

* 3 GLuint (4 bytes each)
* 2 uint16_t (2 bytes each)

---

The size of my mesh on D3D after reordered is 32 bytes, the order is:

* 3 pointer (8 bytes each)
* 2 uint16_t (2 bytes each)
* 4 bytes for alignment

---

The size of my effect on OpenGL after reordered is 16 bytes, the order is:

* 1 GLuint (4 bytes)
* 2 Handle (4 bytes each)
* 1 uint16_t ( 2 bytes)
* 1 RenderState (1 bytes each)
* 1 byte for alignment

---

The size of my effect on D3D after reordered is 48 bytes, the order is:

* 1 RenderState (32 bytes)
* 2 Handle (4 bytes each)
* 1 uint16_t ( 2 bytes)
* 6 byte for alignment

---

### sDataRequiredToRenderAFrame

The size of my sDataRequiredToRenderAFrame is 172 bytes on OpenGL, 184 bytes on D3D. In the struct, a `eae6320::Graphics::ConstantBufferFormats::sPerFrame` is already there. I put a float[4] to store background color, two pointers to store the list of pointers of meshes and effects, a uint8_t to store the size of pairs stored. The memory that the pointers point at is not included.

| Size(byte) |     sDataRequiredToRenderAFrame |
|:--------|:-------:|:--------:|:--------:|:-------:|:--------:|--------:|
| OpenGL   | 172   | 1 eae6320::Graphics::
ConstantBufferFormats::sPerFrame (144 bytes)   | 2 pointers (4 bytes) | 1 float[4] 16 bytes | 1 uint8_t (1 byte) | 3 bytes for alignment
|----
| D3D   | 184   | 	1 eae6320::Graphics::
ConstantBufferFormats::sPerFrame (144 bytes)   | 2 Pointers (8 bytes)| 1 float[4] 16 bytes | 1 uint8_t (1 byte) | 7 bytes for alignment)|
|=====
{: rules="groups"}

## Controls
Press and hold <kbd>SPACE</kbd> to make color animation slow. Release to return to normal speed. 
Press and hold <kbd>F1</kbd> to hide the top triangle, press and hold <kbd>F2</kbd> to change the color of the top triangle to pure white.

## Screenshots

{% capture images %}
	../assets/img/blog/GameEngineering2/Assignment4/1.png
	../assets/img/blog/GameEngineering2/Assignment4/2.png
{% endcapture %}
{% include gallery images=images caption="Default state. Background color will change based on time" cols=2 %}

{% capture images %}
	../assets/img/blog/GameEngineering2/Assignment4/3.png
{% endcapture %}
{% include gallery images=images caption="The top triangle is hidden" cols=1 %}


{% capture images %}
	../assets/img/blog/GameEngineering2/Assignment4/4.gif
{% endcapture %}
{% include gallery images=images caption="The color of the top triangle is changed to white
, the top triangle is hidden" cols=1 %}


## Optional Challenges

### Animate background color

In `Graphics::RenderFrame()`, we can get the simulation time directly in constant data, but in game we can’t. So I added a function `GetElapsedSecoundCount_simulationTime()`, which is similar to `GetElapsedSecondCount_systemTime()` . The function looks for the variable `m_tickCount_simulationTime_totalElapsed` and return it directly. Then we can use it to animate color.

### Change mesh automatically

Override the virtual function named `UpdateSimulationBasedOnTime` then make a triangle blink every 0.1 second.

## Discussion

It’s game engineer’s duty to decrement the reference count when game doesn’t need it anymore. So game engineer should decrement all the reference count in `Game::CleanUp`. If Game doesn’t clean it and Graphics doesn’t have the reference, it will leak memory.