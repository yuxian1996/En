---
layout: post
title:  "Realtime Rendering - PBR"
date:   2019-04-19
excerpt: "Explain PBR and add specular map to our engine"
tag: [EAE6900, Realtime Rendering]
comments: false
---

## [PBR](https://en.wikipedia.org/wiki/Physically_based_rendering){:.no-decoration}
PBR stands for physical based rendering, a way to that more accurately models the flow of light in the real world. [BRDF](https://en.wikipedia.org/wiki/Bidirectional_reflectance_distribution_function) (Bidirectional Reflectance Distribution Function) is the simplified model to describe the properties of the material. BRDF tells us how much light is reflected from input to output for any two directions. \\( f(\omega_i, \omega_o) = f(\omega_o, \omega_i)\\)

The rendering equation of PBR is:

\\[ L_o(\omega_o) =  \int_0^\Omega(\omega_i, \omega_o)L_i(\omega_i)cos\theta_id\omega_i + L_e(\omega_o) \\]

It sums the reflected light from every direction. The \\(cos\theta_i \\) is a non-negative and \\(L_e\\) is the emitter color of this surface if the emitter exists. 

PBR introduces the definition of microsurface. A microsurface BRDF is:

\\[f(\omega_i, \omega_o) = {D(h)F(\omega_i, h)G(\omega_i, \omega_o, h)\over 4cos\theta_i cos\theta_o} \\]

where h is the half vector of input and output, D(h) is Distribution Function, describing how normal of microsurface align with half-normal. \\(F(\omega_i, h)\\) is Fresnel Function. Fresnel effect is an effect that more light is reflected when the view angle is steeper. Fresnel Function is the function we used to calculate how much light is reflected by Fresnel Effect. \\(G(\omega_i, \omega_o, h)\\) is Geometry Function, describing the geometry occlusion and inter-reflection among microsurface. In game, it's usually used to cancel some of the BRDF.

For Lambert material, its BRDF function is:

\\[f(\omega_i,\omega_o) = \frac{R_d}{pi} \\]

For our specular material, \\(D(h) = {x+2\over 2pi}{(h\doth)}^x\\), \\(F(\omega_i, h) = R_0 + [(1 - R_0){(1-(l\doth))}^5]\\),
G(\omega_i, \omega_o, h) = |n\dotv||n\dotl|. The final equation is:

\\[L_0= {(\alpha+2)*(n\doth)^\alpha\over 8}(R_0+(1-R_0)\dot(1-(l,h))^5)(n\dotl)) \\]

### Result

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment13/1.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment13/1.gif"></a>
    <figcaption>Change smoothness dynamically</figcaption>
</figure>

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment13/1.png
    ../assets/img/blog/RealtimeRendering/Assignment13/2.png
{% endcapture %}
{% include gallery images=images caption="Fresnel Effect" cols=2 %}


## Gloss Map
Gloss Map or Specular Map is used to control the smoothness of a material. Gloss map is a single channel texture which has only one float for each texel, so it should be compressed by BC4 Compression. I assume the artists to author these textures using real world reference so the source image is already in linear space and doesn't need to be converted. 

I map the sampler value from [0, 1] to [1, 256] by following equation:
~~~c++
float gloss = pow(2, round(SamplerTexture2d(g_glossMap, g_glossState, i_texcoord) * 8));
~~~

### Result

You can notice that some parts of the material are more smooth than other, especially in the left side.

<figure>
	<a href="../assets/img/blog/RealtimeRendering/Assignment13/2.gif"><img src="../assets/img/blog/RealtimeRendering/Assignment13/2.gif"></a>
    <figcaption>Specular Map</figcaption>
</figure>
