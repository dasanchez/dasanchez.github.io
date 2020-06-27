---
layout: default
---

# Make a Peer-to-Peer Photo Album

I became a father recently. Before baby's arrival, my partner and I agreed on not posting any images of our child to social media. After the little one was born, I started sharing photos with my parents and siblings through WhatsApp.

For the first month or so, I started wondering if I could create a digital photo album that would bypass not only the zuckerverse, but centralized hosting services in general. In this post, I will go over the solution I have in place (so far).

## Why?

First off, why not just make an album on one of my Google Photos/Office 365/Dropbox accounts and share that instead?

I have a few issues with relying on a company to host images of a child that hasn't provided consent to whatever nebulous privacy policies they may offer. Mostly, I want to avoid creating an online fingerprint (be it through name, images, or otherwise) ideally until baby becomes a teenager.

I see the business model of the hosting services I listed above as follows:

1. You take a photo with your phone! A brand new image file is created.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

{:start="2"}
2. You upload your picture to a central hosting service that you may or may not be paying for. Let's call it Goofbox. Goofbox will make sure your data is not lost.

![Photo upload](/assets/img/p2p-photo-album/photo-upload.svg)

{:start="3"}
3. If at some point you want to use Goofbox to share your picture, you can request a link you can share with people so they can access the data.

![Photo share](/assets/img/p2p-photo-album/photo-share.svg)

{:start="4"}
4. As soon as the data arrives in their computers, Goofbox will have the option to sell, *ahem*, monetize as much of it as they can- the image itself, my identity and location, identities and locations of everyone else who has also accessed this image, and whatever data Goofbox may have on us. In my case, hosting my baby pictures in Goofbox may lead to diaper ads showing up on my Goofbox searches. Forget that noise.

![Photo monetization](/assets/img/p2p-photo-album/photo-monetize.svg)

### What is the alternative?

I tried to visualize different models that would let me keep better ownership and control of my data.


### Option A: Run a Photo Management Package on a Dedicated Server

I could opt for one of many existing solutions for private photo albums out there and run it on my own server in the cloud (e.g. a Digital Ocean droplet). The process would then look like this:

1. You take a photo with your phone! A brand new image file is created.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

{:start="2"}
2. You upload your picture to a server you control and maintain. You are responsible for data backups.

![Upload photo to my server](/assets/img/p2p-photo-album/photo-upload-my-server.svg)

{:start="3"}
3. If at some point you want to share your picture or photo album, you can send the address of the photo server to whoever you want to share it with.

![Share my server address](/assets/img/p2p-photo-album/photo-share-my-server.svg)

A quick Goofbox search coughs up the following:

- [Piwigo](https://piwigo.org/)
- [Chevereto](https://chevereto.com/)
- [Lychee](https://lychee.electerious.com/)
- [PicApport](https://www.picapport.de/en/index.php)

With most of these, I would have to learn some PHP, SQL, or Java. I'm focused on getting better at C++ and Python right now, so I don't feel like throwing another language in the mix.

Also, I would have to either pay for a server or sort out access to one of my home computers if I wanted to dedicate one to this task.

❌ Heavy installation and maintenance requirements  
❌ Pay for dedicated server

### Option B: Share Files Directly from my Computer

A different approach to relying on a single server is to have my relatives download the latest version of the photo album directly from my computer:



**This is a peer-to-peer system!**
 
I am aware of two (somewhat  new) protocols for peer-to-peer file sharing: IPFS and Dat.

IPFS is made to share all sorts of media, but the tools I could find do not allow for user-friendly updates to a given set of files. I want a single address that I can reuse even after I have added new photos, for instance. IPFS lets you do this through the IPNS system, but it's not straightforward to me.

Dat is more geared towards data sets. Beaker (a web browser powered by Dat) is built for self-publishing websites and web apps.

As I was reading up on my options, I came across one of Paul Frazee's live streams: [Build a personal website in Beaker with WebComponents](https://www.youtube.com/watch?v=M7g_ZCqSI88). In it, he builds a simple website that updates a Photos page whenever he adds images to a local folder. The dat protocol then allows these changes to propagate to any computer that access the dat URL. If these machines have "seeding" on, then they can serve the content themselves! This way my computer doesn't need to stay on the whole time.

I thought that if I could adapt his code, I could build a photo album website that would be easy enough to update, and the biggest challenge afterwards would be to get my relatives to install a new, unfamiliar browser.

✔ New browser and a little bit of HTML5 know-how  
✔ I can publish a website from any personal computer  
⚠ Kinda so-so: I need to provide storage space

Here is the idea so far:

1. You take a photo with your phone! A brand new image file is created.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

2. You copy the image file to your computer (I don't think a solid dat browser exists for mobile *yet*). 


3. Add the file to your hyperdrive.


4. Send the hyper:// address to whoever you want to share the drive with.


5. One relative accesses the drive and they start browsing the photo album.


6. Shortly after, another relative starts browsing the photo album as well and they start hosting it too

7. If your computer goes offline, relative # 2 can still serve the content from their computer. Amazing!

This has the added benefit of redundant data backups. If my computer breaks, I can recover the data as long as there is at least another machine online seeding the hyperdrive.


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

