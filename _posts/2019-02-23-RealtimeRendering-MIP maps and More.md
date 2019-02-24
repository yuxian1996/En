---
layout: post
title:  "Realtime Rendering - MIP maps and More"
date:   2019-02-21
excerpt: "Exaplain what's Mip map, how does it work. Add more effect with texture UV and alpha value."
tag: [EAE6900, Assignment, Realtime Rendering]
comments: false
---

## Filtering
Texture is just an array of data which contains color information. The number of texels and the number pixels are rarely same, so different ways to calculate color of a pixel from texture will lead to different color results. 

The default behavior of GPU without filtering is to get the color of nearest neighbor texel by UV. If the camera is really close to a texture, each texel overlaps with a block of pixel, having a clear edge with other adjancent texels.

If we enable a bilinear filtering, GPU will find several texels around the position in texture coordinate and get the average color of those texcel colors. It has a smoother and more realistic result when camera is really close to texture.

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment07/disable.png
    ../assets/img/blog/RealtimeRendering/Assignment07/enable.png
{% endcapture %}
{% include gallery images=images caption="Texture without filtering / with bilinear filtering" cols=2 %}


## MIP Maps
MIP maps are precomputed sequence images of a original image, each of them has a lower resolution of the original image. Using MIP maps could improve runtime performance by reducing sampler number. It also reduces aliasing at large distance.

<figure>
Highest level of MIP map
	<a href="../assets/img/blog/RealtimeRendering/Assignment07/high MIP.png"><img src="../assets/img/blog/RealtimeRendering/Assignment07/high MIP.png"></a>
</figure>

In the first image, MIP maps are enabled, everything looks normal. In the second image, MIP maps are disabled, we can notice there is distortion in the face-up texture. Because GPU maps the position in image coordinate to texture coordinate, if we render a large texture to a small block of pixels, two adjancent pixels might be mapped to two positions far from each other, which might cause aliasing. 

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment07/MIP enabled.png
    ../assets/img/blog/RealtimeRendering/Assignment07/MIP disabled.png
{% endcapture %}
{% include gallery images=images caption="Texture with/without MIP maps" cols=2 %}

The size of MIP maps are divided by 2 each time. If the original texture is 256 x 256, its MIP maps contains 8 images:
128 x 128, ,64 x 64, 32 x 32, 16 x 16, 8 x 8, 4 x 4, 2 x 2, 1 x 1.

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment07/mip0.png
    ../assets/img/blog/RealtimeRendering/Assignment07/mip1.png
    ../assets/img/blog/RealtimeRendering/Assignment07/mip2.png
    ../assets/img/blog/RealtimeRendering/Assignment07/mip3.png
    ../assets/img/blog/RealtimeRendering/Assignment07/mip4.png
    ../assets/img/blog/RealtimeRendering/Assignment07/mip5.png
{% endcapture %}
{% include gallery images=images caption="Part of MIP maps" cols=2 %}
