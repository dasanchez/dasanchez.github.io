---
layout: default
---

# Laser Engrave a Wallet

For my first post, I will outline a workflow for laser engraving using [Inkscape](https://inkscape.org) with an [Epilog Helix](https://www.epiloglaser.ca/products/legend-laser-series.htm) laser system in mind.
 
I have an [aluminum wallet](https://www.ogondesigns.com/en/) I love (thanks dad!), and shortly after joining [Newmakeit](http://www.newmakeit.com) I figured I would use the Helix to add a custom engraving to it. If you have an aluminum piece that is either anodized or powder-cated, it's ready for engraving (I may be oversimplifying things a bit).

I'm a Sigur Rós fan and really like the birds motif they use in some of their [artwork](https://s-media-cache-ak0.pinimg.com/originals/62/b5/76/62b57607d1e6bd04354403149f46df0e.jpg), so I set out to engrave a flock of birds and my name on the wallet.
 
### Bitmap source
I grabbed some free clipart from the web:

![Bird bitmap](/assets/img/01/bird_bitmap.jpg)

It's rather low-res, but that will work out since I am looking for a minimalistic, simplest-shapes feel.
 
### Name source
I captured my handwritten name with my Surface and saved a PNG of it.
 
## Inkscape
I start a new document and set the document properties to 24" x 18" in Landscape mode to match the laser platform.
 
![Document Properties](/assets/img/01/01_DocProps.PNG)

Then I need to set the usable area. My wallet measures 108mm x 73mm, so I create a layer called _Usable area_ and draw a rectangle at the top right corner with those dimensions. I also add a "keep out" area that represents the wallet logo in the corner.

![Usable area](/assets/img/01/02_Usable area.PNG)
 
With my frame in place, I create another layer to drop the artwork in (_File->Import_). I have to flip and invert the image to get the birds flying  in the right direction and avoid the logo.

![Artwork](/assets/img/01/03_Artwork.PNG)
 
I select _Path->Trace Bitmap_, and use Brightness cutoff in the single scan section.

![Brightness cutoff](/assets/img/01/04_Cutoff.PNG)

I can now move this trace to an _Engraving_ layer.

![Name](/assets/img/01/05_Trace.PNG)

 
I follow the same steps above to bring in and trace the handwritten name.

![Full engraving](/assets/img/01/06_Engraving.PNG)


All that is left is to hide the _Usable area_ and _Artwork_ layers before saving the file as a PDF. I open the file in Adobe Reader and do a quick check on the shapes and outlines, mostly making sure the "construction" layers are not visible.

![PDF QC](/assets/img/01/07_PDF.PNG)

Send it to the Helix using the recommended manufacturer settings, and five minutes later we've got this:

![Final product](/assets/img/01/08_Final.jpg)
 
The cloudiness is from extended use (less than stellar anodizing I guess).

Questions? Comments? [Give me a shout!](/about)
