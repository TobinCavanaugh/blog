Neural Radiance Fields (NeRFs) are some of the coolest up and coming tech in the world of rendering and spatial capture. Photogrammetry is decent, but NeRFs fill a new niche, that being having detailed 3D captures that include material data and light simulation.

A great application for NeRFs would be in capturing art. I recently went to the Prada Museum in Madrid, Spain. It was a fantastic experience, and after two and a half hours my head and feet hurt from seeing so much art. I didn't get to see everything I wanted to see there, and when I went afterwards to look up photos of certain art pieces, I couldn't find good quality ones. Photos aren't allowed in the Prada Museum (though I took a few sly ones) so my memory of the art is the only way I can experience a lot of that art again.

The solution is NeRFs, by scanning these art pieces with high resolution cameras and outputting a high resolution NeRF, we could open up some of the greatest works of art ever to the student. I would love to go back to the Prada Museum, but I just don't have the money or time, and I think most people are in the same boat. Opening up this art to the rest of humanity would only have good effects, and I think it would make the world a better place.

Why NeRFs in particular? Besides the fact that the tech is convenient, NeRFs maintain things that would normally be lost by regular photogrammetry or regular photography. An obvious one is the brush strokes and how they reflect the light. Most paintings are not purely 2D, most have brush strokes and paint that lift off of the canvas, leading to a deeper view of the painting. When a 2D photo or even a photogrammetry scan is taken, this is lost. The frames are also something that is lost by 2D photos. Many paintings I saw had unique framing schemes, particularly Bartolomé Bermejo's *Saint Dominic of Silos enthroned as a Bishop*.

```image-layout-a
---
caption: Saint Dominic of Silos enthroned as a Bishop
descriptions:
- Without frame
- With frame
---
![[https://github.com/TobinCavanaugh/blog/blob/master/_assets/Saint%20Dominic%20of%20Silos%20enthroned%20as%20a%20Bishop.jpg?raw=true]]

![[https://github.com/TobinCavanaugh/blog/blob/master/_assets/Santo_Domingo_de_Silos_entronizado_como_obispo,_por_Bartolom%C3%A9_Bermejo.jpg?raw=true]]
```
> These are from [Wikipedia](https://en.wikipedia.org/wiki/Bartolom%C3%A9_Bermejo#/media/File:Santo_Domingo_de_Silos_entronizado_como_obispo,_por_Bartolom%C3%A9_Bermejo.jpg) and the [Prado Museum Website](https://www.museodelprado.es/en/the-collection/art-work/saint-dominic-of-silos-enthroned-as-a-bishop/f4cd7ad1-cc50-48fe-86f5-71dfe6672db1), respectively.

As you can see the same painting with the frame included and without it included are very different experiences. Both of these, however, are still 2D photos, and don't at all capture the true scale and 3D of the ornate frame. The best solution to expressing the entirety of the painting is via a NeRF.

As I mentioned earlier, the loss of a dimension is more significant than one might immediately assume. Beyond just the frame and whatnot, we also lose all of the experience of the light. An important one to me is the *Vista de Bermeo* by Paret y Alcazar Luis, which you can see in my photo below. A similar image can also be found at the [Museo Del Prado Website](https://www.museodelprado.es/en/the-collection/art-work/view-of-bermeo/e5c8ce62-39a8-443f-8f43-1b0ac6917d74).

| <center>![Image](https://github.com/TobinCavanaugh/blog/blob/master/_assets/Pasted%20image%2020250320210253.png?raw=true)</center> |
| :--------------------------------------------------------------------------------------------------------------------------------- |
| <center>*Vista de Bermeo* by Paret y Alcazar Luis</center>                                                                         |

This is not a painting, but rather a type of mosaic named *Commesso* or *Florentine mosaic* [^src]. What's lost in this image is the way the light reflects the stone. At a distance like the photo, the piece almost looks like a painting, but at a glancing view, you see the sheen of the individual stones. Each type of stone, while smooth, has a distinct look at this angle.

This is entirely lost in the photo, in fact it would require numerous different photos at different angles to express. The solution, of course, is NeRFs.

A great demo of what this tech can achieve in the lighting space is on [Matthew Tancik's blog](https://www.matthewtancik.com/nerf). This can be seen in the following video, where the TV screen reflects the light accurately. The right side demonstrates that the light can be viewed independently of the camera angle.

<video controls loop="true" muted="">
<source src="http://cseweb.ucsd.edu/~viscomp/projects/LF/papers/ECCV20/nerf/website_renders/viewdirs_website_bww.mp4">
</video>

[^src]: https://en.wikipedia.org/wiki/Commesso