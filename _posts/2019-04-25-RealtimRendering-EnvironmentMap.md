---
layout: post
title:  "Realtime Rendering - Environment Map"
date:   2019-04-25
excerpt: "Use cube map to simulate the environment around our object as environment map"
tag: [EAE6900, Realtime Rendering]
comments: false
---

## Environment Map

So far the light sources we use to render our scene are only directional light and point light, while in real world, lighting bounces everywhere. The diffuse color of a surface is the color bounced out from the surface. If an object has specular reflection, we should be able to see the reflected environment as a mirror in specular reflection. However, we don't have the information of other object in our scene, thus an environment map is used to represent the approximate environment. 
In our engine, environment map is basically a 3D cube map, which has 6 faces (textures) as a cube, using a 3D vector (from center of cube to the sampler point) to get the sampler color.

### Reflection
The reflected color comes from the environment map, sampling by the reflected direction of the view direction. We use the same formula as we used in [PBR]({{"/RealtimRendering-PBR" | prepend: site.baseurl}}) to calculate reflected light, except the dot product in Fresnel function should be normal dot reflected direction. The reflection should be additive but it would look too bright if we add it directly since the color we get from environment map is only an approximate light color without attenuation, occlusion and anything else. In our engine, multiplying the reflection by diffuse light (light dot normal) before adding it would get a better result. 

### Result

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment14/1.png
    ../assets/img/blog/RealtimeRendering/Assignment14/2.png
{% endcapture %}
{% include gallery images=images caption="Gold Material" cols=2 %}

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment14/3.png
    ../assets/img/blog/RealtimeRendering/Assignment14/4.png
{% endcapture %}
{% include gallery images=images caption="Non-metal Material" cols=2 %}