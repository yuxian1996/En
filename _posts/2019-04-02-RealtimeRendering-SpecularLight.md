---
layout: post
title:  "Realtime Rendering - Specular Light"
date:   2019-04-02
excerpt: "Add specular light"
tag: [EAE6900, Realtime Rendering]
comments: false
---

## Specular Light
Specular light is the light reflected directly to our eyes by surface. To calculate the specular light of a point on surface, we need to get the light source from all directions, which costs a lot time to calculate in real games. Also since the light source in game is simplified to a single direction or point, we can observe the specular light as one single point only if we're in the direction of reflection. Thus we need a way to simulate specular light quickly.

The basic idea of Phong Lighting and Blinn-Phong Lighting is that the fragments around the perfect reflection point also reflect some dimmer specular light to our eyes. The light intensity decreases as the angle between view direction and surface normal increases. We'll see a round highlight in this simulation.  

Because specular light is based on the direction of light direction **AND** view direction, it will change if one of those two directions changes. 


### Phong Lighting
In Phong Lighting, the intensity of the light we observe is defined by light direction, view direction, surface normal and a exponent value alpha. The equation to calculate specular color is: 
\\[Light color = s * (| R \cdot V |)^\alpha\\]

where R is the reflect direciton from surface, V is the view direction from surface to eye, s is a color value defining the reflective of a material, alpha is the exponent ot define how specular the surface is. 

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/phong.png"><img src="../assets/img/blog/RealtimeRendering/Assignment11/phong.png"></a>
</figure>

### Blinn-Phong Lighting

Equation: 
\\[Light color = s * (| N \cdot H |)^\alpha\\]

The equation of Blinn-Phong Lighting is similar to Phong Lighting, except that we use the dot product of N and H instead of R and V, where H is the direciton of half vector between V and L.

### Results
<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/lightchanging.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment11/lightchanging.gif"></a>
    <figcaption>Change light direction</figcaption>
</figure>

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/camerachanging.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment11/camerachanging.gif"></a>
    <figcaption>Change camera position</figcaption>
</figure>

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment11/ex64.PNG
    ../assets/img/blog/RealtimeRendering/Assignment11/ex32.PNG
    ../assets/img/blog/RealtimeRendering/Assignment11/ex16.PNG
    ../assets/img/blog/RealtimeRendering/Assignment11/ex8.PNG
{% endcapture %}
{% include gallery images=images caption="exponent of 64, 32, 16, 8" cols=2 %}

## Point Light
Point light is different with directional light, which is simplified as a position other than a direction. The light direction of point light is caculate by fragment position minus light position.

Point light has an attenuation information to decrease its intensity by the distance between fragment position and light position. 
<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment11/pointlight.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment11/pointlight.gif"></a>
    <figcaption>Change point light position</figcaption>
</figure>