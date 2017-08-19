---
layout: default
---
[Home](/)

# 3D Print a Snap-Fit Piece

I was recently tasked with outfitting and organizing a brand new mechanical assembly room at work.
I picked up a Husky garage tool rack from Home Depot to hang up hand tools. It looks like [this](https://www.homedepot.ca/en/home/p.4-feet-x-4-feet-track-wall.1001003414.html).

They offer a number of accessories to go with it, including hooks and a [rack with square holes in it](https://www.homedepot.ca/en/home/p.steel-shelf.1001004370.html).
This would be great for hanging all sorts of drivers if the openings weren't so large. I set out to print out some reducing adapters.

## Concept
My idea is simple: make a snap-fit "adapter" that sits on (and locks itself within) a square opening, but exposes a much smaller opening for flat, cross, and hex drivers. I have worked with enough [FDM](https://en.wikipedia.org/wiki/Fused_deposition_modeling) pieces to know that there is quite a bit of give with printed [ABS](https://en.wikipedia.org/wiki/Acrylonitrile_butadiene_styrene), so a "snap-fit" piece will not be particularly challenging.
The geometry will only require the following features:
1. A base for the head of the driver
2. Two "flaps" that lock against the bottom of the rack.
3. A channel for the driver that will also serve as a pilot hole if I need to drill it open.

![Concept](/assets/img/06/01_CONCEPT.PNG)

## Reducing Adapter MKI

The first model looks like this in Fusion 360:

![MKI Model](/assets/img/06/02_MODEL_MKI.PNG)
![MKI Front](/assets/img/06/03_FRONT_MKI.PNG)

Using SolidWorks Simulation I confirm that it will take under 10N to flex both tabs all the way in. So, after ten minutes on the MakerBot at [Newmakeit](http://www.newmakeit.com) I end up with this:

![MKI Print](/assets/img/06/04_PRINT_MKI.JPG)

The tabs do the job alright, but I left a bit too much clearance against the square shape so the whole thing rattles a bit much.

## Reducing Adapter MKII

The second model is only a bit wider, and the spacing for the rack sheet metal goes down from 1.5mm to 1mm:

![MKII Model](/assets/img/06/05_MODEL_MKII.PNG)
![MKII Model](/assets/img/06/06_FRONT_MKII.PNG)

[This is what the piece looks like on the tool rack](http://i.imgur.com/T2rJJUL.gifv). It's a much better fit now.

## Lessons learned

You can easily make snap-fit pieces using hobby-grade 3D printers. You just have to experiment :)

Questions? Comments? [Give me a shout!](/about)

[Home](/)





