---
layout: default
title: Getting Started
---

Let's start by setting up a new Framework  project and creating the files for our application.

## Download & Install

Please see the instructions in the [framework README &raquo;](https://github.com/Famous/framework#setup--installation)

## Setup

    $ famous framework-scaffold
    ? Enter your component's name: apple-tv
    ? Does the project name "my-name:apple-tv" look ok? Yes
    (Lots of installation messages...)

Once everything is installed, create three new files in the apple-tv folder named  `apple-tv.css`,`apple-tv.html` `galleryData.js`. To do this from the command line, you can type:

    $ cd components/my-name/apple-tv
    $ touch apple-tv.css
    $ touch apple-tv.html 
    $ touch galleryData.js
    $ cd ../../..

If done correctly, your folder structure should now look similar to this:
    
     apple-tv-lesson
      ├─ ...
      └─components/
         └─ my-name/
             └─ apple-tv/
                └─ apple-tv.css
                └─ apple-tv.html
                └─ apple-tv.js
                └─ galleryData.js


The `apple-tv.js`  file will contain our entire [Framework component](#), `apple-tv.html` will contain the [Tree](../tree.html), `galleryData.js` will contain our the data for our images, and, as you might have guessed, `apple-tv.css` will contain our external styles. 

Running the command `npm run dev` from the terminal will let you view the project's live output on [http://localhost:1618](http://localhost:1618) as you make changes to it. Entering `control` + C into the terminal will exit. Open the project up in your favorite text editor before moving on to the next step. 

<span class="cta">
[Up next: Structuring an app &raquo;](./structuring-an-app.html)
</span>
