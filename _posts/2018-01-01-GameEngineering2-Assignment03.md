---
layout: post
title: "Game Engineering 2 - Assignment03"
date: 2018-01-01
excerpt: "This is the writeup for assignment03, EAE 6320 'Game Engineering 2'"
tags: [EAE6320, Game Engine, Assignment]
comments: false
---

<div markdown="0"><a href="https://drive.google.com/open?id=1xCuBPiRuN3qXcy--2oTnF3O6TWJBzwWZ" class="btn btn-info">Download</a></div>

## Content

In this assignment, we are required to make Graphics system completely platform-independent and make mesh and effect be initialized by specific data.

I decided to add another namespace named PlatformSpecific to deal with platform specific functions and variables. The main differences of two platform are initialization, cleanup, clear color, clear depth, and swap back buffer to front buffer. 
The declarations of these interfaces are located in PlatformSpecific.h, the implementations of these interface are located in `PlatformSpecific.gl.cpp` and `PlatformSpecific.d3d.cpp` separately. The interfaces I created for PlatformSpecific are listed below:

1. 
~~~ c++
Result Initialize(const Graphics::sInitializationParameters& i_initializationParameters);
~~~ 
This interface takes a parameter because we need it to initialize something in D3D.

2. 
~~~ c++
cResult CleanUp();
~~~

3. 
~~~ c++
void ClearColor(const float r, const float g, const float b, const float a = 1.0f); 
~~~
This interface takes r, g, b, a as back color.

4. 
~~~ c++
void ClearDepth(const float iDepth = 1.0f); 
~~~
This interface takes a depth value to be set as cleared depth.

5. 
~~~ c++
void Swap();
~~~

#### Graphics uses the statement below to clear back buffer color
~~~ c++
PlatformSpecific::ClearColor(sin(simulationTime), cos(simulationTime), cos(simulationTime));
~~~

#### Graphics uses the statement below to initialize an effect

The initialization of effect takes the path of vertex shader and fragment shader to create a new effect.

~~~ c++
s_effect1.Initialize("data/Shaders/Vertex/standard.shader", "data/Shaders/Fragment/LoopColor.shader");
~~~


| Size(byte) | cEffect |
|:--------|:-------:|:--------:||:-------:|:--------:|--------:|
| OpenGL  | 16   | 1 GLuint(4 bytes)   | 2 Handle(4 bytes each) | 1 RenderState(1 byte | 3 bytes for alignment|
|----
| D3D   | 40   | 2 Handle(4 bytes each)   | 1 RenderState(32 bytes)|
|=====
{: rules="groups"}


#### Graphics uses the statement below to initialize an mesh
The initialization of mesh takes a vector of vertex data and a vector of index data. The type of mesh1 is `std::vector<VertexFormats::sMesh>`, the type of indices1 is `std::vector<uint16_t>`. 

| Size(byte) | cEffect |  |
|:--------|:-------:|:--------:||:-------:|--------:|
| OpenGL  | 16   | 3 GLuint(4 bytes each)   | 1 uint16_t(2 byte) | 2 bytes for alignment |
| D3D   | 32   | 3 Pointer( 8 bytes each)	| 1 uint16_t(2 bytes)| 6 bytes for alignment|
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
{: rules="groups"}

---

Yet I didn’t find a way to make my cMesh and cEffect even smaller. In future assignment we might have to store more data into our mesh and effect class, I guess we could list them from top to bottom as larger data to smaller data to decrease the memory for alignment.

## Controls
Press and hold <kbd>SPACE</kbd> to make the color change slowly. Release <kbd>SPACE</kbd> to return to normal speed.

## Screenshots

{% capture images %}
	../assets/img/blog/GameEngineering2/Assignment3/1.png	
	../assets/img/blog/GameEngineering2/Assignment3/2.png	
	../assets/img/blog/GameEngineering2/Assignment3/3.png	
	../assets/img/blog/GameEngineering2/Assignment3/4.png	
	../assets/img/blog/GameEngineering2/Assignment3/5.gif	
{% endcapture %}
<!-- {% include gallery images=images caption="Game Engineering2 - Assignment03" cols=1 %} -->
{% include gallery images=images cols=1 %}
