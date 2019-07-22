---
layout: default
---

# 3D Print a Baseball Case

How about we build a slick Blue Jays-branded baseball case?

## Concepts
At first I thought of scultping a blue jay head that would seat a baseball within it:

![Concept 1](/assets/img/03/01_CONCEPT.PNG)

But honestly, that felt like way too much work. Instead, I figured a semi-spherical shaped case would suffice.

![Concept 2](/assets/img/03/02_CONCEPT.PNG)

## Artwork Preparation: Inkscape
I bring a Blue Jays logo into Inkscape, and tweak the settings so that an outline trace separates the maple leaf from the rest of the shape. I do this mostly so that *painting* is not too challenging later on.

![BJ Logo](/assets/img/03/03_LOGO.PNG) ![BJ Logo & Trace](/assets/img/03/04_LOGO_TRACE.PNG) ![BJ Trace](/assets/img/03/05_TRACE.PNG)

After a quick check of the generated paths, I export the file as a DXF so I can use it in a CAD environment.

## Modelling the case: Fusion 360
Google tells me that a standard baseball has a diameter of 76mm, so I give myself a roomy 100mm-diameter spherical envelope.  
I import the DXF and extrude the relevant shapes past the top face of the sphere.

![Modelling 01](/assets/img/03/06_LOGO_EXTRUDE.PNG)

Add a flat base, and a 78mm-diameter spherical cavity.

![Modelling 02](/assets/img/03/07_INSIDE_CAVITY.PNG)

Cut the case in half, and at this point I notice I need some sort of mechanism to keep the top on. I had told myself "hinge" earlier on, but I will have to either add an off-the-shelf hinge or print my own- and again, I get lazy.  
I opt instead for a push-and-turn scheme. Using two diametrically opposed pins on the bottom piece and two mirrored paths on the top one I can provide a secure enough way to join the two:

![Modelling 03](/assets/img/03/08_BOTTOM_PINS.PNG) ![Modelling 04](/assets/img/03/09_TOP_PATH.PNG)

I add two channels to the bottom piece for finger clearance and save a render for posterity before heading out to Newmakeit:

![Render](/assets/img/03/10_RENDER.PNG)

## Printing: Cubicon
It has been a while since my last visit to Newmakeit, and they have added a Cubicon 3D printer recently. Looking at the sample pieces on display, it looks like it can produce some fine quality prints. I throw the base at it first, and overjoyed at the results, print the top part next.

Alas, I have overlooked a flat portion within the model and added no supports for it. I end up with a large spaghetti area inside the piece:

![Bad geometry](/assets/img/03/11_BAD_GEOMETRY.jpg)

After carving quite a bit of material, I manage to join both halves. I can work with this.

![Finished print](/assets/img/03/12_PRINT.jpg)

## Finishing: Sanding and Painting
I have used glow-in-the-dark filament again, so I decide to finish the top part only (it's the one with the logo on it anyway). I will follow this checklist:

1. 100-, 200-, 400-grit sanding
2. Gesso layer + 24hr cure
3. 200-, 400-grit sanding
4. Gesso layer + 24hr cure
5. 200-, 400-grit sanding
6. Paint

I pick up some Liquitex gesso and acrylic paints at my local art supply, get to work.

One day after applying the first layer of gesso:

![Gesso 1](/assets/img/03/13_GESSO1.jpg)

Shortly after applying second layer of gesso:

![Gesso 2](/assets/img/03/14_GESSO2.jpg)

The final product:

![Finished piece](/assets/img/03/15_PAINTED.jpg)

LET'S GO BLUE JAYS!

### Lessons learned

1. I could have gone harder on the 100-grit sanding. The layer lines are pretty visible at the top of the case.
2. Don't forget to double check your geometry before sending files to the 3D printer. The printer might be great, but it can't do miracles.

Questions? Comments? [Give me a shout!](/about)
