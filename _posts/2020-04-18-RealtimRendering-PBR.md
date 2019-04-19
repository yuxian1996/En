---
layout: post
title:  "Realtime Rendering - PBR"
date:   2019-04-19
excerpt: "Explain PBR and add specular map to our engine"
tag: [EAE6900, Realtime Rendering]
comments: false
---

## PBR[https://en.wikipedia.org/wiki/Physically_based_rendering]
PBR stands for physical based rendering, a way to that more accurately models the flow of light in the real world. BRDF[https://en.wikipedia.org/wiki/Bidirectional_reflectance_distribution_function] (Bidirectional Reflectance Distribution Function) is the simplified model to describe the properties of the material. BRDF tells us how much light is reflected from input to output for any two directions. \\{ f(\omega_i, \omega_o) = f(\omega_o, \omega_i)\\}

The rendering euqation of pbr is:

\\[ L_o(\omega_o) =  \int_0^\Omegaf(\omega_i, \omega_o)L_i(\omega_i)cos\theta_id\omega_i + L_e(\omega_o) \\]

It sums the reflected light from every direction. The \\{ cos\theta_i \\} is a non-negative and \\{L_e\\} is the emitter color of this surface if the emitter exists. 

PBR introduces the definition of microsurface. A microsurface BRDF is:

\\[f(\omega_i, \omega_o) = {D(h)F(\omega_i, h)G(\omega_i, \omega_o, h)\over 4cos\theta_i cos\theta_o} \\]

where h is the half vector of input nad output, D(h) is Distribution Function, describing how normal of microsurface align with half-normal. \\{F(\omega_i, h)\\} is Fresnel Function. Fresnel effect is an effect that more light is reflected when the view angle is steeper. Fresnel Function is the function we used to calculate how much light is reflected by fresnel effect. \\{G(\omega_i, \omega_o, h)\\} is Geometry Function, describing the geometry occlussion and inter-reflection among microsurface. In game, it's usually used to cancel some of the BRDF.

For Lambert material, its BRDF function is:

\\[f(\omega_i,\omega_o) = \frac{R_d}{pi} \\]

For our specular material, \\{D(h) = {x+2\over 2pi}{(h\doth)}^x\\}, \\{F(\omega_i, h) = R_0 + [(1 - R_0){(1-(l\doth))}^5]\\},
G(\omega_i, \omega_o, h) = |n\dotv||n\dotl|. The final equation is:

\\[L_0= \\]




### Add Normal Map to Pipeline


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

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment12/2.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment12/2.gif"></a>
    <figcaption>Good looking normal maps</figcaption>
</figure>
