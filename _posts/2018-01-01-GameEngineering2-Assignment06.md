---
layout: post
title: "Game Engineering 2 - Assignment06"
date: 2018-01-01
excerpt: "This is the writeup for assignment06, EAE 6320 'Game Engineering 2'"
tags: [EAE6320, Game Engine, Assignment]
comments: false
---

<div markdown="0"><a href="https://drive.google.com/open?id=1Q6MGfHCKERNACKo9U7idELvXYpzih56E" class="btn btn-info">Download</a></div>

## Content

In this assignment, we need to move the mesh data which is hard-coded before into a human-readable asset file.

### Advantage of Human-readable Asset Files

Human can read the information from a human-readable asset and understand what each data means. We can create or change a human-readable asset file easily and use it to test our program.

### Mesh File Example

Mesh has two dictionaries, we can understand what kind of data each table has from its name.
VertexArray has a list of arrays, each array means the coordinates of one vertex. Because our coordinates only has 3 floats, the length of an array is 3 and contains float data. IndexArray also has a list of arrays, each array means the index data of one triangle. It’s determined by us that we draw a mesh by triangle, so each array has only 3 integer to present the index data.
In this case, we can get VertexArray and IndexArray by name, then read the data of each table by index. It’s understandable to who has a little knowledge of graphics

~~~ ruby
return 
{
   VertexArray = 
   {
        { -1.0, -1.0, 1.0},
        { -1.0, 1.0, 1.0},
        { 1.0, 1.0, 1.0},
        { 1.0, -1.0, 1.0},
        { -1.0, -1.0, -1.0},
        { -1.0, 1.0, -1.0},
        { 1.0, 1.0, -1.0},
        { 1.0, -1.0, -1.0},
   },

   IndexArray = 
   {
	-- front
        { 0, 2, 1},
        { 2, 0, 3},
	-- back
		{ 5, 6, 4},
        { 7, 4, 6},
	-- up
        { 1, 2, 5},
        { 2, 6, 5},
	-- down
        { 4, 3, 0},
        { 4, 7, 3},
	-- left
        { 1, 5, 4},
        { 1, 4, 0},
	-- right
        { 6, 2, 7},
        { 7, 2, 3}
   }
}
~~~

### Explicit Dependency

BuildMyAssets needs a MeshBuild.exe to be there before it can be built, so we have to add an explicit dependency of MeshBuilder to BuildMyAssets.

## Controls

Press <kbd>W</kbd><kbd>A</kbd><kbd>S</kbd><kbd>D</kbd> to move the cube. Hold Control and press <kbd>W</kbd><kbd>A</kbd><kbd>S</kbd><kbd>D</kbd> to move the triangle. Press <kbd>Up</kbd><kbd>Down</kbd><kbd>Left</kbd><kbd>Right</kbd> to move camera.

## Screenshots

<figure>
	<a href="../img/blog/GameEngineering2/Assignment6/1.png"><img src="../img/blog/GameEngineering2/Assignment6/1.png"></a>
</figure>
