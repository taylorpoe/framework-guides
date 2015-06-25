---
layout: default
title: Positioning images
---

Our images are evenly dispersed within the `#rotator-node`, but we want start them off below the screen. To do this, we will create a helper function in our `galleryData.js` file to add random initial positions for the vertical animations.

## Understanding the coordinates

Since we rotated our main `#rotator-node`, the coordinates used for positioning the `#gallery-items` are also flipped by 90 degrees. This means that modifying the Z coordinates (not the Y coordinates) will move our image nodes up and down and the Y coordinates now control the depth. 

![zposition](zaxis.png)

If understanding this is a bit confusing, think of it as if we're now looking at the red box above ( `#rotator-node` ) from a bird eye's view and we need to position the children (`#gallery-items`) from this angle. 

## Animating the nodes up

We want to start our images off the screen and slowly move them up.  To do this, we will start at a positive Z value off the screen (window height + random starting point) and then slowly decrement that Z value to move them up. 

Currently X and Y are randomly dispersed using: `Math.random()*500`. However, we can't use this same function for Z since we want to slowly increment the value without the `Math.random()` being called on every frame. Instead, we will initialize random Z values in our state using a helper function. 


## Helper function to the rescue

Open up `galleryData.js` and add the following lines below the `imageData` array:
      
      //store window size before the scene is loaded
      var contextSize = window.innerHeight

	  function randomCoordinates(imageData){
	      var result = [];
	      for(var i=0; i < imageData.length; i++){
	      //start outside of the viewing window and random disperse below
	        result.push(Math.floor(contextSize/2+100+Math.random()*contextSize*2))
	      }
	      return result;
	  }

We will use this function to create an array of random Z positions outside of the frame. Next, update the state value for `positionZ` so it calls on this function passing in our imageData. This will give us an array of random Z positions for each item in our image data.

	states: {
	      rotationValue: 0,
	      srcs: imageData,
	      contextSize: contextSize, //update this value as well
	      positionZ:randomCoordinates(imageData)
    }
    
We also update our context size to the `contextSize` reference so the items will fill the stage.

Great! We have everything set up to animate our images. In the next section, we will add events to do just that. 

<span class="cta">
[Up next: Creating animations &raquo;](./animations.html)
</span>
