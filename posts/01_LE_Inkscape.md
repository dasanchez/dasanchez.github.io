---
layout: default
---
[Home](/)

# Laser Engraving with Inkscape

For my first post, I will outline a workflow for laser engraving using Inkscape and an Epilog Helix laser system.
 
My dad bought me an aluminum wallet during a trip to Japan, and shortly after joining [Newmakeit](http://www.newmakeit.com) I figured I would use the Helix to add a custom engraving to it. These aluminum wallets are fairly popular now- all you need is an anodized piece to do some laser engraving.]

I love the birds motif Sigur Ros uses in some of their [artwork](https://s-media-cache-ak0.pinimg.com/originals/62/b5/76/62b57607d1e6bd04354403149f46df0e.jpg), so I set out to engrave a flock of birds and my name on the wallet.
 
### Bitmap source
I grabbed some free clipart from the web:

![Bird bitmap](/assets/img/01/bird_bitmap.jpg)

It's rather low-res, but that will work out since I am looking for a minimalistic, simplest-shapes feel.
 
### Signature source
I captured my handwritten name with my Surface and saved a PNG of it.
 
## Inkscape
I start a new document and set the document properties to 24" x 18" in Landscape mode.
 
![Document Properties](/assets/img/01/01_DocProps.PNG)

Then I need to set my usable area. My wallet measures 108mm x 73mm, so I create a layer called _Usable area_ and draw a rectangle at the top right corner with those dimensions. I also add a "keep out" area to represent the wallet logo in the corner.

![Usable area](/assets/img/01/02_Usable area.PNG)
 
With my frame in place, I create another layer to drop the artwork in (_File->Import_). I have to flip and invert the image to get the birds flying  in the right direction and avoid the logo.

![Artwork](/assets/img/01/03_Artwork.PNG)
 
I select _Path->Trace Bitmap_, and use Brightness cutoff in the single scan section.

![Brightness cutoff](/assets/img/01/04_Cutoff.PNG)

I can now move this trace to an _Engraving_ layer.

![Signature](/assets/img/01/05_Trace.PNG)

 
I follow the same steps above to bring in and trace the signature.

![Full engraving](/assets/img/01/06_Engraving.PNG)


All that is left is to hide the _Usable_ area" and _Artwork_ layers before saving the file as a PDF. I open the file in Adobe Reader and do a quick check on the shapes and outlines, mostly making sure the "construction" layers are not visible.

![PDF QC](/assets/img/01/07_PDF.PNG)

Send it to the Helix using the recommended manufacturer settings, and five minutes later we've got this:

![Final product](/assets/img/01/08_Final.jpg)
 
The cloudiness is from extended use (less than stellar anodizing I guess).

