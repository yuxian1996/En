---
layout: post
title:  "Realtime Rendering - Specular Light"
date:   2019-0-05
excerpt: "Add spe"
tag: [EAE6900, Realtime Rendering]
comments: false
---

## Specular Light
Specular light is the light reflected directly to our eyes by surface. To calculate the specular light of a point on surface, we need to get the light source from all directions, which is hard to calculate in real game. Also since the light source in game is simplified to a single direction or point, we can observe the specular light only if we're in the direction of reflection. Thus we need a way to simulate specular light quickly.

### Phong Lighting
In Phong Lighting, the intensity of the light we observe is defined by light direction, view direction, surface normal and a exponent value alpha. The equation to calculate specular color is: 

`Light color = s * | R V | alpha `

where R is the reflect direciton from surface, V is the view direction from surface to eye, s is a color value defining the reflective of a material, alpha is the exponent ot define how specular the surface is. 

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/phong.png"><img src="../assets/img/blog/RealtimeRendering/Assignment11/phong.png"></a>
</figure>

### Blinn-Phong Lighting

`Light color = s * | N H | alpha`
The equation of Blinn-Phong Lighting is similar to Phong Lighting, except that we use the dot product of N and H instead of R and V, where H is the direciton of half vector between V and L.

### Results
<figure>
Change light direction
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/lightchanging.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment11/lightchanging.gif"></a>
</figure>

<figure>
Change camera position
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/camerachanging.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment11/camerachanging.gif"></a>
</figure>

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment11/ex64.png
    ../assets/img/blog/RealtimeRendering/Assignment11/ex32.png
    ../assets/img/blog/RealtimeRendering/Assignment11/ex16.png
    ../assets/img/blog/RealtimeRendering/Assignment11/ex8.png
{% endcapture %}
{% include gallery images=images caption="exponent of 64, 32, 16, 8" cols=2 %}

## Point Light

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/pointlight.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment11/pointlight.gif"></a>
</figure>