---
layout: post
title: "Game Engineering 2 - Assignment01"
date: 2018-01-01
excerpt: "This is the writeup for assignment01, EAE 6320 'Game Engineering 2'"
tags: [EAE6320, Game Engine, Assignment]
comments: false
---

<div markdown="0"><a href="https://drive.google.com/open?id=19zQYNnEb5ZXd7K2YLszgi3Ghbe7ts1bn" class="btn btn-info">Download</a></div>

## Requirements

This is the solution setup for our game engine. In order to understand how to a large program and build the architecture, we practiced the things below:

1. Add new project to a solution
2. Manage project property by property sheet
3. Figure out dependencies of each project and add static libraries (dependencies) and external libraries.
4. Learned platform-independent interfaces by using platform-specific code underlying.
5. Learned how to use Visual Studio to manage project in all means.

I followed all the instructions JP gave to us one by one to set up my solution. I got a link error which told me I can’t find d3d lib or opengl lib, later I figured out that I forgot to force include win.h files in the new Graphics project I added.
I changed the title and icon enum in cMyGame.h. As for custom shader, I used `sin(simulationTime)` and `cos(simulationTime)` to calculate the color of my triangle and normalize them to (0,1).

## Controls
Press and hold <b>SPACE</b> to make the color change slowly. Release <b>SPACE</b> to return to normal speed.

## Reference
Project Application is needed to add a reference to Graphics.
Project ShaderBuilder mentioned Graphics namespace but didn’t need a reference to Graphics. Because they are all enumerations other than functions defined in a CPP file.

## Class Expectations
I like graphics and I’ve learned some basic theory of graphics before. I feel it’s too hard to research new theory in graphics but it’s still cool to build our own game engine and rendering system by mature technology. I’d like to the more about graphics and real-time rendering in games.

## Optional Challenge

* There’s a file named “shaders.inc” which should be included by all shaders. We only need to add code which declares constant buffers to this header file. (I tested it but I don’t add it to the final commit because I don’t think it will be included in all shaders in the future.)
* The function UserInput::IsKeyPressed take a keycode (enumeration) and return if it’s pressed. There’s a member variable in cbApplication named m_simulationRate, which is used to calculate simulation time in cbApplication::UpdateUntilExit. Then the value is submit to engine. We only need to use the function SetSimulationRate and pass a number in [0, 1) to make it slower.
Combine those above, we can detect key press in cMyGame::UpdateBasedOnInput and change simulation rate.


## Screenshots

<figure>
	<a href="../img/blog/GameEngineering2/Assignment1/screenshot.png"><img src="../img/blog/GameEngineering2/Assignment1/screenshot.png"></a>
</figure>
