# Benchmark map for WorkAdventure

Knowing the limitations of the software you use is important. That's why I created this project.

The goal was to create an insanely large map to see what were the software limitations that WorkAdventure uses to create and run maps.
This includes:
* [Tiled](https://www.mapeditor.org/)
* [GitHub](https://github.com/)
* [Phaser](https://phaser.io/)
* A web browser (I'm using Chrome)

## Logs
 
### Test #1: 100,000x100,000 tiles

So I started by saying to myself: let's create a map of 100,000x100,000 tiles.

In Tiled I therefore entered 100,000 ti... wait, it's limited to 5 characters. This was fast.

### Test #2: 99,999x99,999 tiles

Ok, so let's try 99,999x99,999.

I entered that, and I validated, the grid appeared (we do not discern that it's a grid, it's more a mixture of black lines ^^).
Quite surprised by the fact that Tiled was still standing. The fluidity of the zoom level is impressive. I thought to myself "ok let's add some ground and save it all".
I saved. I waited about 15 seconds. 🎶 Bye bye birdy 🎶, Tiled was gone. We're going to have to be nicer to him...

### Test #3: 9,999x9,999 tiles

I restarted Tiled, I entered 9,999 in horizontal and vertical, I saved, I waited ... BOOM, Tiled was KO again.

Okay, from now on, let's proceed by dichotomy.

### Test #4: 5,000x5,000 tiles

I had high hopes for this 5,000. This time I looked at my RAM consumption when I was saving.
No luck, another crash while my consumption was at 14 GB (just by Tiled).

Next: 2,500... Well, I have less and less hope, so let's say 2,000 to have better luck.

### Test #5: 2,000x2,000 tiles

Same story: I saved, I waited ... and let's go! The file is saved after 10 seconds of long suspense!

I could try to increase the dose, but I really wanted to see the result in WorkAdventure.

With 2k it makes already... 2,000 x 2,000 tiles = 4,000,000 tiles = 128,000,000 pixels. Wow. Remember when I wanted to try 100k?...
What I found impressive was how Tiled was rendering the painting of 4,000,000 tiles at once, quickly and without flinching! 
Here is a little souvenir:

![2k-map](./public/2k-map.png)

Ok, some stats before my commit/push:
* Opening this file with Tiled will consume 7 GB of RAM
* The JSON file size is 145 MB
* It takes 10 seconds to save this big boy

Now, it's GitHub time!

I add the file, I committed "Biggest map ever made - muwhahaha!".
I pushed, and after a few seconds I saw some red text. I hate it when there is red text in my terminal.
It was telling me the file was too big. GitHub doesn't want it, we have to go with a solution called "Git LFS" (for Large File Storage).
These Githubians have planned everything.

So I looked at the documentation, tried to install and use it (it's hell). 12 years later, I finally managed to push my repository. OK now no more surprises... OK?
It turned out that using LFS is not possible for us because of one of the GitHub actions (thextruding program) that wants a real JSON file, not just a reference to a file (that's how LFS works).

In conclusion, the file must be less than 100 MB. So the plan is to cut the map in Tiled until we get it.

### Test #6: 1,600x1,600 tiles

After 3-4 attempts, I arrived at a result of 1600x1600 tiles for a file that goes under the 100 MB mark (~ 90 MB).
Before pushing one last time, I took the design of the [starter kit](https://github.com/thecodingmachine/workadventure-map-starter-kit) repo (it's a small office) which I duplicated over the entire width and height of the map. This small setup contains 2 Jitsi areas and enough furniture for my test.

![adding-offices](./public/adding-offices.gif)

Just to give you an idea here, this map contains about 47 of these small offices across the width and 88 across the height, making no less than 4,136 offices ready to kick in the doors of WorkAdventure!

Little update to the stats:
* Opening this file with Tiled will now consume less than 2 GB of RAM
* The JSON file size is 93 MB
* It takes 6 seconds to save this big boy

Now what I want to do is to add this damn stat:
* It takes X minutes for the player to travel the entire map horizontally

So here we go: I pushed, the GitHub actions are now working, I waited a bit for GitHub Pages to be OK, I opened the deployments view, I clicedk on my [WorkAdventure link](https://play.workadventu.re/_/this-is/valdotr.github.io/largest-map-ever/map.json)! ... aaaand the winner is... SIGILL. Yep, that's his real name:
 
![SIGILL](./public/SIGILL.png)

So now it's probably Phaser's turn to get in my way... the world is crap, really.

### Test #7: 1200x1200 tiles

Seven is my favorite number. It has to work.

I had the feeling that the next step will work, so I used a different strategy: I started with a solution that I knew it will work, increasing the number until having the final word. So I started with 600x600, then 800x800, then 1200x1200 and finally 1400 crashed the same way as 1600.
Let's analyze our map with 1200x1200 tiles:

* Opening this file with Tiled consumes a negligible amount of RAM
* The JSON file size is 52 MB
* It takes 6 seconds to open the file and 3 seconds to save it
* It takes 3'33" for the player to travel the entire map horizontally
* It takes 15 seconds to load the map in WorkAdventure

![inside_wa](./public/inside_wa.png)

We are not finished yet. Now that the number of tiles is not an issue anymore, let's increase the number of layers!
The current map has 12 layers.

### Test #7.1: 1200x1200 tiles / 100 layers

I created a copy of one of my layers that I copied 82 times... to have my total of 100 layers.

![copies](./public/copies.png)

When I saved Tiled was about to crash, I had to force wait for the program to save my work.
I looked at the file size I saw 433 MB. Ok so already knew that I had to remove a bunch of layers to pass below 100 MB (LFS issue).
But I ran the map anyway and I saw the SIGILL error. Too bad.

### Test #7.1: 1200x1200 tiles / 22 layers

I reduced the number of layers (each layer contains the walls for the whole map) by removing 50 of them. The file was way to big yet, so I kept removing, and removing... and with a total of 23 layers my file size was just under the 100 MB limit (99,9 MB).

So I ran this map in WorkAdventure: still have the memory error code "SIGILL".

### Test #7.2: 1200x1200 tiles / 14 layers

Ok, this part is super interesting. I removed more copies of my walls until having a total of 14 layers (15 layers gave me the memory error crash).

![3-copies](./public/3-copies.png)

With this quantity of memory to manage, Chrome stopped the execution of the Phaser code just before having to crash! Maybe he did that because I had the console open.

![chrome-debug-phaser](./public/chrome-debug-phaser.png)

In the memory tab, I saw a heap snapshot of 3900 MB, so I assume we get the out-of-memory error when morethan 4 GB of data is allocated to the Chrome Javascript objects.

Before removing one layer, I wanted to know if the memory allocation was the same with layers full of tiles and layers empty (or with just one tile). So I removed all my copies of walls and I created one layer with one tile, that I copied several times. The result is that having many tiles drawn on a layer or not is not impacting the performance of the game. Only the number of layers counts, alongside the number of tiles of course.

> It's the number of tiles that counts, not the number of pixels. So if we had a grid of 16x16 tiles we had double the size of our map.

Now I think we can see the size of a map this way, in a cuboid shape:

![cuboid](./public/cuboid.png)

So to have an idea of the real number of tiles that Phaser must work with, we have to see them in 3D: `1200 x 1200 x 14 = 20,160,000` elements of 32x32 pixels.

### Test #7.2: 1200x1200 tiles / 13 layers

An update about the number of elements with 13 layers: `1200 x 1200 x 13 = 18,720,000` elements.

This is working with Chrome. With Firefox I had to remove some layers to make it working. It worked with 11 layers but the FPS was quite unstable.

I think that if we used a more opimized way of allocating the memory for each elements, we could have a way larger map.

## Conclusion

We have seen what were the limitations of each piece of software. At the end, we had a map too large to be able to create many layers, but 99.9% of the time you will never have to be careful about how many layers you create.
That being said, I never had to create more than 25-30 layers to create my maps.

Usually a "big map" in WorkAdventure is around 300x300 tiles. Maybe in the future people will have the need to create huge maps (with metaverses). We will then have to edit the game engine itself to be able to optimize it, or potentially we simply don't use 100% Phaser's capabilities. We will investigate this!

## Config

Tested with this PC config:
- OS: Ubuntu 20.04.1 LTS
- RAM: 32 GB
- CPU: Intel Core i7 - 7700HQ @ 2.80GHz x 8
- GPU: Mesa Intel HD Graphics 630 (KBL GT2)

## Installation and licenses

See the repository of the template used to create this map: https://github.com/thecodingmachine/workadventure-map-starter-kit