---
layout: default
---

# Make a Peer-to-Peer Photo Album

I became a father recently. Before baby's arrival, my partner and I agreed on not posting any images of our child to social media. After the little one was born, I started sharing photos with my parents and siblings through WhatsApp.

For the first month or so, I started wondering if I could create a digital photo album that would bypass not only the zuckerverse, but centralized hosting services in general. In this post, I will go over the solution I have in place (so far).

First off, why not just make an album on one of my Google Photos/Office 365/Dropbox accounts and share that instead?

I have a few issues with relying on a company to host images of a child that hasn't provided consent to whatever nebulous privacy policies they may offer. Mostly, I want to avoid creating an online fingerprint (be it through name, images, or otherwise) ideally until baby becomes a teenager.

I see the business model of the hosting services I listed above as follows:

1. I take a photo with my phone! A brand new image file is created.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

{:start="2"}
2. I upload my picture to a central hosting service that I may or may not be paying for. Let's call it Goofbox. Goofbox will make sure my data is not lost.

![Photo upload](/assets/img/p2p-photo-album/photo-upload.svg)

{:start="3"}
3. If at some point I want to use Goofbox to share my picture, I can request a link to share with people so they can access the data.

![Photo share](/assets/img/p2p-photo-album/photo-share.svg)

{:start="4"}
4. As soon as the data arrives in their computers, Goofbox will have the option to sell, *ahem*, monetize as much of it as they can- the image itself, my identity and location, identities and locations of everyone else who has also accessed this image, and whatever data Goofbox may have on us. In this case, hosting my baby pictures in Goofbox may lead to diaper ads showing up on my Goofbox searches. Forget that noise.

![Photo monetization](/assets/img/p2p-photo-album/photo-monetize.svg)

### What is the alternative?

I tried to visualize different models that would let me keep better ownership and control of my data.

## Option A: Run Photo Management Software on a Dedicated Server

I could pick one of many existing solutions for private photo albums out there and run it on my own server (e.g. a [Digital Ocean](https://www.digitalocean.com/) droplet). The process would then look like this:

1. I take a photo with my phone.

![Baby photo](/assets/img/p2p-photo-album/baby-photo.svg)

{:start="2"}
2. I upload the picture to a server I control and maintain. I am responsible for data backups.

![Upload photo to my server](/assets/img/p2p-photo-album/photo-upload-my-server.svg)

{:start="3"}
3. If at some point I want to share a picture or photo album, I can send the address of the photo server to whoever I want to share it with.

![Share the server address](/assets/img/p2p-photo-album/photo-share-my-server.svg)

A quick Goofbox search coughs up the following:

- [Piwigo](https://piwigo.org/)
- [Chevereto](https://chevereto.com/)
- [Lychee](https://lychee.electerious.com/)
- [PicApport](https://www.picapport.de/en/index.php)

With most of these, I would have to learn some PHP, SQL, or Java. I am not proficient in any of them, and this project does not motivate me enough to pick up another language right now.

Also, I would have to either pay for a server or sort out access to one of my home computers if I wanted to dedicate one to this task.

❌ Heavy installation and maintenance requirements  
❌ Pay for dedicated server

## Option B: Share Files Directly from my Computer

A different approach to relying on a single server is to have my relatives access the my computer (and the photo album) directly:

![Peer-to-peer sharing](/assets/img/p2p-photo-album/direct-share.svg)

**This is a peer-to-peer system!** Visit the [Wikipedia page for peer-to-peer](https://en.wikipedia.org/wiki/Peer-to-peer) if you want to explore this concept further.

I am aware of two (somewhat new) protocols for peer-to-peer file sharing: [IPFS](https://ipfs.io/) and [Hypercore](https://hypercore-protocol.org/).

**IPFS** is made to share all sorts of media, but I did not find its ecosystem to be all that intuitive for creating and consuming content alike (i.e. updating and accessing a photo album regularly). I am fairly sure IPFS is not the solutionthat I am looking for. 

**Hyperdrive** is a peer-to-peer filesystem built with hypercores. **[Beaker](https://beakerbrowser.com/)**, a peer-to-peer browser, is built for self-publishing websites and web apps through hyperdrives. This is much closer to what I have in mind.

Beaker works as follows:
- You create a hyperdrive that contains a bunch of files. This may be a single video file, a bunch of images, or even a website!
- Beaker creates a unique `hyper://` address for your hyperdrive. You can share this address with other people running Beaker.
- Whenever a computer accesses a `hyper://` address, it becomes a hyperdrive "peer" in the network.
- If a peer chooses to "host" the hyperdrive, they will be able to propagate it to even more computers. This means, for example, that if my sister decides to host a copy of my photo album, I can turn my computer off once she has received the latest updates, and my mom can get the content from my sister (assuming she has her computer on)!

Beaker 1.0 was released recently, and the developers even provided a [template for photo album hyperdrives](https://beaker.dev/docs/templates/photo-album/)!

So, Option B is definitely my favourite one so far:
  
✔ I can publish a website from any personal computer  
⚠ Requires a special browser and some web programing know-how  
⚠ Kinda so-so: I need to provide storage space

This last point is critical in terms of data ownership: with this model, I become the sole owner of the data, but at the same time I alone am responsible for the storage media it lives on (hard drives, USB drives, SD cards, and so on). 

Here is a scenario of how this works:

1. Set up a photo album website in Beaker and link it to a local folder.

I have prepared a template based on the one provided by Beaker. You can go to [this GitHub repo](https://github.com/dasanchez/photo-collection-hyperdrive) to get the files.

![Beaker screenshot](/assets/img/p2p-photo-album/beaker-photo-collection.png)

{:start="2"}
2. Take a photo and copy it to the folder linked to the hyperdrive.

![Baby photo on website](/assets/img/p2p-photo-album/photo-website.svg)

{:start="3"}
3. Share the `hyper://` address with all my relatives. As soon as they open the address in Beaker, they will download the latest version from my computer.

![Share the hyper address](/assets/img/p2p-photo-album/share-hyper.svg)

{:start="4"}
4. Mom closes Beaker after looking at a few photos.

![Mom turns computer off](/assets/img/p2p-photo-album/mom-shutdown.svg)

{:start="5"}
5. My sister starts hosting the hyperdrive.

![Sister turns hosting on](/assets/img/p2p-photo-album/sister-host.svg)

{:start="6"}
6. I add another photo: my sister's laptop gets the update, and I proceed to close Beaker.

![New photo added](/assets/img/p2p-photo-album/hyper-new-photo.svg)

{:start="7"}
7. Mom can now get the latest photos from my sister next time she opens the site. Amazing!

![Mom gets photo update](/assets/img/p2p-photo-album/sister-mom-update.svg)

This setup does have the added benefit of redundant data backups (assuming you have peers willing to host your bytes). If my computer breaks, I can recover the data as long as there is at least another machine online hosting the hyperdrive.

## Bonus: Hosting on a BeagleBone

Now, what if I need a bit more availability and I don't want to dedicate a full-blown computer to serving the website? I dusted off a [BeagleBone Green Wireless](http://beagleboard.org/green-wireless) I had lying around, and I have been using it as a "Beaker mirror" for the last couple of months.

I followed the following steps once I had network access to the Beaglebone:

1. Set up a microSD card as external storage.

I flashed the BeagleBone in such a way that the onboard flash (4GB) worked as the main boot drive, and followed the instructions by Miles B. Dyson to [add a microSD card as extra storage](https://linuxpropaganda.wordpress.com/2018/10/10/beaglebone-black-add-microsd-as-extra-storage-in-ubuntu-server/).

At this point, I have an 8GB microSD card available in the `/media/microsd/` folder.

Make a `hyperdrive` folder in the microSD folder:
```
dante@beaglebone:/media/microsd$ mkdir hyperdrive
```

{:start="2"}
2. Upgrade `node.js`.

Install the `n` module for `node.js`:

```
dante@beaglebone:/media/microsd$ sudo npm install -g n
```

`n` is a tool that allows me to run different versions of node.js on the same computer.

Then, set node.js to version 12.18.2 (that is the stable release version for ARM as I write this):

```
dante@beaglebone:/media/microsd$ sudo n 12.18.2
dante@beaglebone:/media/microsd$ node --version
```
The second command helps me verify that I am running the right version.

{:start="3"}
3. Install [`dat-store`](https://github.com/datproject/dat-store).

```
dante@beaglebone:/media/microsd$ sudo npm install -g dat-store --unsafe
```
I had to use the `--unsafe` flag so it would install without errors.

{:start="4"}
4. Run `dat-store`.

Create a new `tmux` session and run `dat-store` inside it:
```
dante@beaglebone:/media/microsd$ tmux
dante@beaglebone:/media/microsd$ dat-store run-service --storage-location /media/microsd/hyperdrive --expose-to-internet
```

Detach from the tmux session with `Ctrl+B` followed by the `D` key. Then, add the hyperdrive to the `dat-store`:
```
dante@beaglebone:/media/microsd$ dat-store add hyper://[hyperdrive key]
```

Once the BeagleBone has synced to the latest version of the photo album, I am free to turn off my computer! The BeagleBone will stay on to make the files accessible to whoever requests the hyperdrive.


## What's next?

I would like to make the following upgrades:

- **Add photos through a web interface**

Instead of copying files from my phone to a local folder, I would like to have a web portal at home I can upload files to directly from my phone.

- **Allow other family members to contribute their images**

This is the most complicated of the bunch, and goes hand-in-hand with the previous point. I will need to really think this one through and probably get more familiar with the Beaker/Hyperdrive API to find out how much of a challenge it would be.

# Thanks

- To the folks at the [Dat Foundation](https://dat.foundation/) and [Beaker Browser](https://beakerbrowser.com/) for all the work you have put into these projects
- To [RangerMauve](https://github.com/RangerMauve) for all the help and tips around Beaker and dat mirrors
- To [Zach!](https://coolguy.website/) and [cameralibre](https://github.com/cameralibre) for introducing me to the wonderful world of SVG animations (yes, this is partly your doing!)

---

Questions? Comments? [Give me a shout!](/about)