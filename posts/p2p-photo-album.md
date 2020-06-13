---
layout: default
---

# Make a Peer-to-Peer Photo Album

I became a father recently. Before baby's arrival, my partner and I agreed on not posting any images of our child to social media. After the little one was born, I started sharing photos with my parents and siblings through WhatsApp.

For the first month or so, I started wondering if I could create a digital photo album that would bypass not only the zuckerverse, but centralized hosting services in general. In this post, I will go over the solution I have in place (so far).

## Peer-to-peer: What? Why? How?

First off, why not just make an album on one of my Google Photos/Office 365/Dropbox accounts and share that instead?

I have a few issues with relying on a company to host images of a child that hasn't provided consent to whatever nebulous privacy policies they may offer. Mostly, I want to avoid creating an online fingerprint (be it through name, images, or otherwise) ideally until baby becomes a teenager.

I see the business model of the hosting services I listed above as follows:

1. You take a photo with your phone! A brand new image file is created.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

{:start="2"}
2. You upload your picture to a central hosting service that you may or may not be paying for. Let's call it Goofbox. Goofbox will make sure your data is not lost.

![Photo upload](/assets/img/p2p-photo-album/photo-upload.svg)

{:start="3"}
3. If at some point you want to use Goofbox to share your picture, you can request provides you with a link you can share with people so they can access the data.

![Photo share](/assets/img/p2p-photo-album/photo-share.svg)

{:start="4"}
4. As soon as the data arrives in their computers, Goofbox will have the option to sell, *ahem*, monetize as much of it as they can. In my case, hosting my baby pictures in Goofbox may lead to diaper ads showing up on my Goofbox searches.



Being a known and visible target, Goofbox may be exposed to hacks or leaks by either external or internal actors. This could lead to my baby photos becoming public. I would not like that.

**What is the alternative?**

I tried to visualize different models that would let me keep better ownership and control of my data.


### Option A: Photo Management Package on Dedicated Server

[svg begins]
1. i take photo of baby
2. i upload photo to own server via fancy web ui
3. show large internet backdrop again
4. parents access custom url (photos.p2pbaby.com), both from their computer or from their phone

[svg ends]

I could opt for one of many existing solutions for private photo albums out there and run it on my own server in the cloud (e.g. a Digital Ocean droplet). The process would then look like this:

1. You take a photo with your phone! A brand new image file is created.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

{:start="2"}
2. You upload your picture to a server you control and maintain. You are responsible for data backups in case something happens to it.

![Photo upload](/assets/img/p2p-photo-album/photo-upload.svg)

{:start="3"}
3. If at some point you want to share your picture or photo album, you can generate a link you can share with people so they can access the data.

![Photo share](/assets/img/p2p-photo-album/photo-share.svg)


A quick Goofbox search coughs up the following:

- Piwigo
- Chevereto
- Lychee
- PicApport

With most of these, I would have to learn some PHP, SQL, or Java. I'm focused on getting better at C++ and Python right now, so I don't feel like throwing another language in the mix.

Also, I would have to either pay for a server or sort out access to one of my home computers if I wanted to dedicate one to this task.

❌ Heavy installation and maintenance requirements
❌ Pay for dedicated server

### Option B: Share Files Directly from my Computer

A different approach to relying on a server is to use a peer-to-peer system. This means that instead of the files being only available on a single machine, my relatives can download the latest version of the photo album and they can in turn make it available to more people:

SVG goes here


I am aware of two protocols for peer-to-peer file sharing: IPFS and Dat.

IPFS borrows a lot of ideas from BitTorrent, but it can be a bit demanding on computer resources. Because it operates on a "global namespace", I would be making my files available on a single, huge filesystem.
Dat is geared towards data set publishing, and has a built-in version control system. Beaker (a web browser powered by Dat) is built for self-publishing websites and web apps.

As I was reading up on my options, I came across one of Paul Frazee's live streams: [Build a personal website in Beaker with WebComponents](https://www.youtube.com/watch?v=M7g_ZCqSI88). In it, he builds a simple website that updates a Photos page whenever he adds images to a local folder. The dat protocol then allows these changes to propagate to any computer that access the dat URL. If these machines have "seeding" on, then they can serve the content themselves! This way my computer doesn't need to stay on the whole time.

I thought that if I could adapt his code, I could build a photo album website that would be easy enough to update, and the biggest challenge afterwards would be to get my relatives to install a new, unfamiliar browser.

✔ New browser and a little bit of HTML5 know-how 
✔ I can publish a website from any personal computer
⚠ Kinda so-so: I need to provide storage space

Here is the idea so far:

[svg begins]
1. take a picture of baby
2. load picture on computer
3. hit Publish button
4. send dat url to family
5. sister downloads website
6. i turn my computer off
7. sister visits parents and they get the website over local wifi
[svg ends]

1. You copy images to a local folder on your computer.
2. You publish a photo album website using the Beaker browser.
3. You share the dat URL with whoever you want.
4. Whoever has access to the URL has the option to seed the site. This way they can provide the data if your computer is off.
5. If your computer breaks, you can always recover the data if there is at least one more machine seeding the site.

## The code

I will only go over the WebComponents- and Dat-related bits in the code. If you want to grab a copy, I have made the source available in GitHub...and Dat, obviously!


Let's say I have two sets of photos, one of old photos and another one of new ones. The structure of the site is as follows:

```
/ index.html
/ new-photos.html
/ old-photos.html
/ components
    layout.js
    photo-grid.js
    ...
/ new-photos
    new_photo_1.jpg
    new_photo_2.jpg
/ old-photos
    old_photo_1.jpg
    old_photo_2.jpg
/ thumb-gen.py   
```

### `index.html`

The website body is assembled through WebComponents:

```
 <body>
    <album-layout>
        <section class="bordered padded">
            <header>Welcome</header>
            <main>
              Hello everyone! Don't forget to seed!
            </main>
          </section>
    </album-layout>

  </body>

  <script type="module" src="/components/layout.js"></script>
```

The `album-layout` tag is implemented in the **`layout.js`** file.

### `components/layout.js`

This file defines the `album-layout` element, and this is where we add the links to the two sets of photos:

```
function renderNavLink (pathname, label) {
    if (window.location.pathname === pathname) {
        return `<li>${label}</li>`
    }
    return `<li><a href="${pathname}">${label}</a></li>`
}

shadow.innerHTML = `
    <link rel="stylesheet" href="/components/layout.css"> 
    <div id="layout">
        <div id="layout-left" class="layout-sidecol">
            <ul>
            ${renderNavLink('/', 'Home')}
            ${renderNavLink('/new-photos', 'New Photos')}
            ${renderNavLink('/old-photos', 'Old Photos')}
            </ul>
        </div>
        <div id="layout-center">
        <slot>
        </slot>  
        </div>
    </div>
```

Each entry within the `ul` tag will be converted into a link to the specified path.

### `new-photos.html`

Each "set" of images has its own .html file that specifies the title and the path to look for the images.

```
<body>
  <album-layout>
    <album-photo-grid img-path='2016-05-Toronto'>
    </album-photo-grid>
  </album-layout>

</body>

<script type="application/javascript" src="/components/layout.js"></script>
<script type="application/javascript" src="/components/photo-grid.js"></script>
```

The `album-photo-grid` element is defined in the `photo-grid.js` file.

### `components/photo-grid.js`

The `album-photo-grid` element builds a grid of images by reading all the files inside the `[img-path]/thumbs` folder.

```
class PhotoGridElement extends HTMLElement {
    constructor() {
        super()
        this.photoFilenames = []
        this.shadow = this.attachShadow({ mode: 'open' })
        this.imgPath = this.getAttribute('img-path')
        // this.render();
        this.load();
    }

    async load() {
        var self = new DatArchive(window.location)
        this.photoFilenames = await self.readdir(this.imgPath)
        this.photoFilenames.pop() // remove thumbs folder
        this.render();
    }

    async render() {
        this.shadow.innerHTML = `
        <link rel="stylesheet" href="/components/photo-grid.css">
        <div class="photos-grid">
        ${this.photoFilenames.map(filename =>
            `<a href=${this.imgPath}/${filename}><img src=/${this.imgPath}/thumbs/${filename}></a>`).join("")}
        </div>
        `
    }
}

customElements.define('album-photo-grid', PhotoGridElement)
```

Each `img` element then gets a link to the full-size image in `[img-path]/`.

### `thumbs.py`

A *Python* file??? Well yes, I use this script to generate new thumbails whenever I want to add new images.

1. It reads inside each folder in the site.
2. For every `*.jpg` file it finds, it looks for another file with the same name inside the `thumbs` folder.
3. If it doesn't find a thumbnail file, it uses OpenCV to create it.
 
It does require a `thumbs` folder to be present already.

## Publishing the Site

So how do I make the site public? There are three two steps to it:

1. Hit **Publish** in Beaker
2. Share the url with my family
3. They must access the url **while my computer is on!** 

Because I published the site on my home computer, the computer must remain on as long as no one else is seeding it.

If dad visits the url on his laptop and seeds it though, mom can then in turn download the website from dad, and so on.

The next time I update the website, by say, adding more photos to the `new-photos` folder, then again, my computer must remain on for at least one machine to get the updated content.

Now, what if I need a bit more availability and I don't want to dedicate a full-blown computer to serving the website? I have a couple of single board computers around, so I dusted off the a Beaglebone Green Wireless and have been using it as my "photo album mirror" for the last month.

### Hosting on a Beaglebone

This only took three steps after the initial setup and getting on my local WiFi network:

1. Set up a microSD card as external storage

I flashed the Beaglebone in such a way that the onboard flash (4GB) worked as the main boot drive, and followed the instructions by Miles B. Dyson to [add microSD as extra storage](https://linuxpropaganda.wordpress.com/2018/10/10/beaglebone-black-add-microsd-as-extra-storage-in-ubuntu-server/).

2. Install [`hypercored`](https://github.com/mafintosh/hypercored)

The stock images from Beaglebone with [Node.js](https://nodejs.org/en/) preinstalled, so it only takes one command to get the board ready for Dat!
```
debian@beaglebone:/media/microsd$ sudo npm install -g hypercored
```

3. Set up and run `hypercored`

Setting up only involves creating a file called `feeds` in the folder you want to download hyperdrives:
```
debian@beaglebone:/media/microsd/dat$ touch feeds
debian@beaglebone:/media/microsd/dat$ echo "dat://[my-dat-url]" > feeds
```
At which point I can run hypercored (I [run it inside a tmux session](https://linuxize.com/post/getting-started-with-tmux/) so it will keep running even after I log out):
```
debian@beaglebone:/media/microsd/dat$ hypercored
```

Once the Beaglebone has synced to the latest version of the photo album, I am free to turn off my computer! The Beaglebone will stay on to make the files accessible to whoever requests the dat url.

## What's next?

In no particular order, I would like to make the following upgrades:

- **Create a new set easily**
Right now I have to create a new html file and update the `layout.js` file whenever I want to add a new set of images. I want to make it so that I only have to create a new folder, throw some pictures in there, and the site will automatically generate the relevant links and pages.

- **Add photos through a web interface**
Instead of copying files from my phone to a local folder, I would like to have a web portal avilable at home I can upload files to directly from my phone.

- **Allow other family members to contribute their images**
This is the most complicated of the bunch. I will need to really think this one through and probably get more familiar with the Dat SDK to find out how much of a challenge it would be.

