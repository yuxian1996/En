---
layout: post
title:  "Realtime Rendering - Normal & Light"
date:   2019-03-02
excerpt: "Add normal to our mesh and directional light to the environment"
tag: [EAE6900, Realtime Rendering]
comments: false
---

 <div markdown="0"><a href="https://drive.google.com/open?id=1ZyPRUs3394yYWSIcCTOPJpCQ7wOgzk0Q" class="btn btn-info">Download</a></div>

## Normal
The normal of a plane is the vector which is perpendicular to every vector in this plane. In real world, one plane has one normal, but in our rendering pipeline, we don't have a shader corresponding to a plane, thus normal is part of vertex data. Different ways to calculate normal for vertex would influence the effect which we'll discuss later.

### Add Normals to Our Pipeline
* Add 3 floats to our VertexFormats.h
* Add layout description for normal in cMesh.d3d.h
* Add normal to our vertexInputLayout.shader and use NORMAL as its semantic

## Light
### Diffuse Light
When a beam of light goes into a surface, some of the light is reflected to another direction by the surface, while some of it is absorbed. The light absorbed by surface is what we called diffuse light.

When a beam of light (<span style="color:orange">orange</span> area) come from the direction parallel to the normal of a surface, the area absorbing light (<span style="color:red">red</span> area) is smallest, which means the average light a unit area absorbs is biggest. When the angle between incoming light and normal is getting bigger, the area is bigger and the average light is smaller. 

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment08/1.png
    ../assets/img/blog/RealtimeRendering/Assignment08/2.png
{% endcapture %}
{% include gallery images=images cols=2 %}

A person called Lambert created the equation to calculate diffuse lighting by the angle between normal and incoming light:

`Multiplicative = magnitude of light * magnitude of normal * max(cos(light, normal) , 0)`

The direction of light here is from surface to light source. The min() function here is to make sure incoming light doesn't come from the back side of the surface, which could causes negative values.

If the light and normal are both normalized, it can be simplified to:

`Multiplicative = max(dot(light, normal), 0)`

### Directional Lighting

Lighting in real world is very complicated. It consists of many types of lighting and lighting source. Directional light is a simple simulation of sunlight which is the most important light source in real world at day time. Directional light only has an incoming direction and doesn't have any light point, which means it comes from everywhere with the same direction.

### Add Directional Light
There are three different ways to calculate lighting.
1. Calculate lighting in local space. This requires us to transform lighting from world space to local space for each draw call, store and submit them in constant buffer per draw call, which increases memory of a bucket by numbers of draw call * size of a vector.
2. Calculate lighting in world space. This requires us to transform normal from local space to world space in vertex shader.
3. Calculate lighting in view space. This requires us to transform normal from local space to view space in vertex shader and transform light direction to view space once in CPU time.

I choose to calculate lighting in world space. It seems that adding light to constant buffer per draw call takes too much memory and the calculating is in CPU time. While the third one does one more operation (transform light direction to view space) than the second one.

### Ambient Lighting
Ambient lighting is a simulation of the lighting reflected by environment in real world. It adds a dim light to every fragment to make it not totally black.

### Result
<figure>
    The spheres in bottom left and top right are not affected by lighting.
	<a href="    ../assets/img/blog/RealtimeRendering/Assignment08/1.gif"><img src="    ../assets/img/blog/RealtimeRendering/Assignment08/1.gif"></a>
</figure>

## Smooth Normal and Hard Normal
Because normal in our pipeline is a data for vertex and interpolated for each fragment, if the mesh we use is low-poly geometry, we need to use soft normal to make an edge soft.  
Hard normal uses the normal of its triangle directly. If the normal of adjacent triangle is pretty different, we'll see an obvious edge between them. Soft normal uses an average normal among all the normals of the triangles this vertex belongs to.

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment08/3.gif
    ../assets/img/blog/RealtimeRendering/Assignment08/2.gif
{% endcapture %}
{% include gallery images=images caption="Hard normal and soft normal" cols=2 %}

### Control
Press <kbd>Up</kbd><kbd>Down</kbd><kbd>Left</kbd><kbd>Right</kbd> to move move up/down/left/right.

Press <kbd>W</kbd><kbd>S</kbd> to move forward and backward.

Press <kbd>Q</kbd><kbd>E</kbd> to rotate anticlockwise and clockwise.

Press <kbd>I</kbd><kbd>K</kbd><kbd>J</kbd><kbd>L</kbd> to rotate directional light.

