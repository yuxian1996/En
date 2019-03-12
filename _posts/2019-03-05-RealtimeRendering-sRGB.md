---
layout: post
title:  "Realtime Rendering - sRGB"
date:   2019-03-05
excerpt: "Explain linear color and sRGB color. Correct the color of our previous images"
tag: [EAE6900, Assignment, Realtime Rendering]
comments: false
---

## sRGB Space
Human eyes are more sensitive to dimmer light, which means we can distinguish the subtle difference when light is dark,while we cannot tell the difference when the light is relatively bright. In this case, what we thought to be half bright is not actually half bright in real world. HP and Microsoft came up with an standarnd for monitors, printers and the Internet to simulate the color space we see, that is sRGB (standard Red Green Blue). What we output from GPU to monitor are colors in linear space, the monitor converts them into sRGB space then human eyes receive them as the color effect we want.

<figure>
    Blue: linear color  Yellow: sRGB    Red: linea / sRGB
	<a href="    ../assets/img/blog/RealtimeRendering/Assignment09/1.png"><img src="    ../assets/img/blog/RealtimeRendering/Assignment09/1.png"></a>
</figure>

### How to Use sRGB space
1. Change linear color to sRGB color. 
2. Any calculation needs to be done in linear color space. In this case, we need to change any color we use to linear color and send it to our shader.
    * Background color
    * Constant buffer color : light color, material color
    * Vertex color
    * Texture color 
    
    Any color that comes from a digit software is probably in sRGB space because we huamn being is the one who assigns these colors. In our case, we're reading material colors and vertex colors from human-readable file, they are probably needed to be converted to linear space, while background colors and light colors are assigned in code.
    For texture color, we can build them as sRGB color texture so that GPU will convert them to linear color when it's used.

### Issues Using sRGB Space
We'll lose precision in the dard side when we convert color from linear space to sRGB space because in sRGB space it grows to bright very quickly. If we use only 8 bits to store each channel, we'll see obvious banding effect.

However, in our engine, the vertex color is coded as uint8_ts, but we don't have to worry about banding. That's because vertex is usually relativly far way from each other, and the GPU will interpolate the (banding) color of each vertex to a smooth color for each fragment. But for texture color, we directly assign colors to fragments from texture samplers, which are close to each other in texture. If texture has banding, our image probably has banding too.

### Result

{% capture images %}
    ../assets/img/blog/RealtimeRendering/Assignment09/srgb.png
    ../assets/img/blog/RealtimeRendering/Assignment09/before.png
{% endcapture %}
{% include gallery images=images caption = "Image Before / After sRGB Correction" cols=2 %}


    

    