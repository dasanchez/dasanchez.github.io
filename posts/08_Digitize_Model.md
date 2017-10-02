---
layout: default
---
[Home](/)

# Digitize a Physical Model
#### (Adventures in Photogrammetry)

As part of my [LazyMouse](https://github.com/dasanchez/lazy-mouse) project, I am looking for an easy way to capture complex (read: organic) real-life geometry so I can work with it in CAD.

## Source

I have a model (my third attempt at this point) that I formed using some [DAS modeling clay](http://canada.michaels.com/on/demandware.store/Sites-MichaelsCanada-Site/en_CA/Product-Show?pid=M10130210&cgid=845166492).  
Its only relevant features are:
- A concave surface for each digit.
- A thumb resting area where the touch pad will go.
- Two flat surfaces so I it can be used in a vertical or horizontal configuration.

![Concept 1 Collage](/assets/img/08/01_CLAY_COLLAGE.PNG)

This results in a nice comfortable fit for my hand.

<blockquote class="imgur-embed-pub" lang="en" data-id="a/Zogep"><a href="//imgur.com/Zogep"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

## Capture

After a failed attempt at connecting with a proud owner of a 3D scanner, I look into [Agisoft PhotoScan](http://www.agisoft.com/). Their website and a few Youtube videos make it look like I can use this software to generate a mesh that I can import into Fusion 360 for further development using only pictures as input.

### Images

I set the clay model on a piece of black foamcore and take a 2-minute video of it with my phone, going around it and making sure I [capture all major features](https://imgur.com/a/ET51S).

I end up with a 200MB high-quality video that contains all the geometry a computer _should_ be able to handle.

The next step is to extract a reliable set of images from this video: enter Python. As part of my [OpenCV learning experience](https://github.com/dasanchez/opencv_study), I write a program that extracts every 10th frame from a video and scales the image down to 0.5X of its original size. It's nothing fancy, but you can get the source [here](https://github.com/dasanchez/vid2png).

At this point, I have 300 images worth of geometry data.

### PhotoScan Mesh

I activate a trial version of PhotoScan professional, and follow these steps:

#### 1. Add images (every single one of them)

#### 2. Align images

![Cameras aligned](/assets/img/08/02_CAMS.PNG)

#### 3. Build dense cloud

![Dense point cloud](/assets/img/08/03_CLOUD.PNG)

Be ready to let your computer run for a while with this step. It took my FX6300 computer with a 660Ti GPU close to 12 hours to generate the point cloud :O

#### 4. Build mesh

![Mesh](/assets/img/08/04_MESH.PNG)

#### 5. Build texture

This is totally unnecessary for my purposes, but it has a nice "wow" factor to it.

![Texture](/assets/img/08/05_TEXTURE.PNG)

I can now export an OBJ file that I can bring into Fusion 360...

## Instant Meshes

...but not before converting the mesh into a **quad** mesh instead of a triangle one.

Enter [Instant Meshes](https://github.com/wjakob/instant-meshes). This program allows you to convert your triangle mesh (say, a STL file from [Thingiverse](https://www.thingiverse.com/)) into a quad mesh that Fusion 360 will be happier with.

![Quad Meshes](/assets/img/08/06_INSTANT_MESHES.PNG)

After running the PhotoScan mesh through Instant Meshes, I now have an OBJ file with quad meshes in it.

## CAD Verification

I import the quad in Fusion 360 using the Insert->Mesh command:

![Fusion start](/assets/img/08/07_FUSION_IMPORT.PNG)

All I need to do now is trim the quads surrounding my object of interest. After some massaging and trimming operations, I end up with a printable solid:

![Render](/assets/img/08/08_FUSION_RENDER.PNG)

I hollow out the model and send an STL to 3DHubs for printing. The next day I pick up an ABS version of my clay model:

![Print](/assets/img/08/09_PRINT_SMALL.PNG)

<blockquote class="imgur-embed-pub" lang="en" data-id="a/1Yhnm"><a href="//imgur.com/1Yhnm"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

Success!

### Lessons learned

1. That <$200 piece of software you keep frowning your nose at is actually quite powerful!
2. Be mindful to produce good quality source material when you know you have a long workflow or process ahead of you. I had to make three clay models to get to the point where I wouldn't have to spend weeks finessing the resulting 3D model.
3. Clay shrinks...a lot! Don't wait too long to capture your geometry or you'll see some warping.

Questions? Comments? [Give me a shout!](/about)

[Home](/)
