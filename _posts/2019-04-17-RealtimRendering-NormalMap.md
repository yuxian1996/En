---
layout: post
title:  "Realtime Rendering - Normal Map"
date:   2019-04-17
excerpt: "Add normal map to our rendering pipeline to offer more geometry detail"
tag: [EAE6900, Realtime Rendering]
comments: false
---

## Normal Map
In our current rendering pipeline, every triangle represents a plane, every vertex has only one normal. If we want to simulate the geometry of an object in real world, the number of triangles has to be extremely large, which is unacceptable in real-time rendering like games. Normal map is a way to increase the geometry details by giving every fragment a normal rather than adding more vertices, so that it looks like there are small bumps on the surface. 
Normal map is a 2D texture, its color value representing the differential between real normal and normal of the triangle. In our engine, the vector (0, 0, 1) in texture space is always the normal of a particular triangle. Its R component corresponds to positive U, G component corresponds to positive V. Normal maps always looks blueish because its B component (Z axis) are probably not far from 1.

Because normal map represents the bias in the space of each triangle, we need to transform it to world space. Two vectors are necessary to get the transformation of a space so we need at least one more vector to calculate the transformation.  Fortunately Maya can calculate the tangent vector and bitangent vector of a triangle for us. A Tangent vector is typically regarded as one vector that exists within the surface's plane (for a flat surface) or which lies tangent to a reference point on a curved surface. Bitangent vector is calculated by the cross product of tangent and normal. 

The range of vector3 we sample from normal texture is [0, 1.0f], it needs to be converted to [-1.0f, 1.0f] first. Then
a “TBN” matrix is created and used to rotate triangle normal to result normal based on the normal offset from normal map. 
~~~c++
float4 normalMap = (SamplerTexture2d(g_normalMap, g_normalMapState, i_texcoord) - 0.5f) * 2.0f;

const float3x3 TBNMatrix = float3x3(
	nTangent.x, nBitangent.x, nNormal.x,
	nTangent.y, nBitangent.y, nNormal.y,
	nTangent.z, nBitangent.z, nNormal.z
);

float3 result = transform(TBNMatrix, normalMap.xyz)
~~~


### Add Normal Map to Pipeline
* Output tangent and bitangent in our maya plugin. 
* Add tangent and bitangent to vertex format.
* Read tangent and bitangent from human-readable mesh file and convert it to binary.
* Add input layout of tangent and bitangent. Pass to from CPU to GPU.

### Result

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment12/1.png
    ../assets/img/blog/RealtimeRendering/Assignment12/2.png
    ../assets/img/blog/RealtimeRendering/Assignment12/3.png
    ../assets/img/blog/RealtimeRendering/Assignment12/4.png
    ../assets/img/blog/RealtimeRendering/Assignment12/5.png
    ../assets/img/blog/RealtimeRendering/Assignment12/6.png
{% endcapture %}
{% include gallery images=images caption="Pointlight from multiple angles" cols=3 %}

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment12/1.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment12/1.gif"></a>
    <figcaption>Another orientation</figcaption>
</figure>

### Make Our Own Normal Map

I created brick material with normal map. I used a height map as an reference to create the normal map. The surface is rasied higher when the color in height map is brighter and sunken deeper when the color is darker. The coordinate I use is from left to right, bottom to up, (0,0) is located in the bottom left corner. The right edge of a brick should face to positive x and looks redish. The up edge of a brick should face to positive y and looks greenish.

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment12/brick_height.png
    ../assets/img/blog/RealtimeRendering/Assignment12/brick_normal.png
{% endcapture %}
{% include gallery images=images caption="Height Map / Normal Map" cols=2 %}

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment12/2.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment12/2.gif"></a>
    <figcaption>Brick w/ Normal Map</figcaption>
</figure>
