---
layout: default
title: Adding states
---

You can think of states as the "settings" of a module. When a state value is injected into a behavior function, that function will be called whenever that value changes. Any dynamic values or global settings should be placed in the states object.

Let's set up the values in our states object before injecting them into the behavior functions. Replace the empty state object in your Framework component with the object below. 

	 states: {
	      rotationValue: 0,   // value to rotate all of our images  
	      srcs: [1,2,3,4],    // this will store the images srcs
	      contextSize: 500,   // define the gallery's size here
	      positionZ:[]        // store our images' Z positions here
	    } 


Now all of the values above are accessible to our behaviors by simply passing them as arguments to the behavior functions. 

Replace your behaviors in `apple-tv.js` with the object below and read through the inline comments. Pay attention to where and how we include the state values above in our behavior functions below.

	behaviors: {
		      '#root':{
		      	  'style':{ // remove background color! set it in the CSS instead
		      	      'perspective':'1000px'
		      	  }
		      	},
	        '#rotator-node': {
	            'size': function(contextSize){ //grab context size from state
	                return [contextSize, contextSize]
	            },         
	            'align': [0.5,0.5],          
	            'mount-point':[0.5,0.5],
	            'origin':[0.5,0.5], //center the origin so it rotates around its middle   
	            'style': {
	                'background': 'red'
	            },
	            'rotation': function(rotationValue){ // this will drive Z rotation animations
	                //rotate backwards and listen for Z rotation changes
	                return [-Math.PI/2.1, 0, rotationValue] 
	            }
	        },
	        '.gallery-item':{
	            'size': [100,100],  
	            'style':{
	                'background-color': 'blue',
	                'border':'2px solid black'
	            },
	            '$repeat':function(srcs){
	                return srcs  //repeat over srcs array
	            },
	            'position-x': function($index, contextSize){ 
	                return Math.random()* contextSize
	            },
	            'position-y':function($index, contextSize){ 
	                return Math.random()* contextSize
	            },
	            'position-z': function($index, positionZ){ 
	                return positionZ[$index] // this will drive the moving image animations
	            },
	            'rotation': [Math.PI/2,0,0] //rotate images forward
	        }
	    } 


If you save the changes, you should see the new rotated scene. 

## Adding external CSS

You'll notice that we removed the CSS background color of the `#root` node above. If you wish to keep a grey background, open up the `apple-tv.css` file you created in the [getting started step](http://famous.org/framework/AppleTV/getting-started.html) and paste in the following CSS:
 
    body {
      background:grey;
    }

We needed to remove the background color from `#root` because we are rotating everything backwards and into the plane of the `#root` node. If we kept the background color on `#root`, half of the `#rotator` would be hidden by a solid grey plane. 

As a general rule, it's always a best practice to keep your styles contained within external CSS. However, in this lesson we add inline styles for convenience. Later, we will show you how to import this CSS file when we import our images. 

## Understanding the rotation

Famous gives us access to a 3D coordinate system: X axis goes left to right, Y up and down and Z forward and back. Let's break down how we will leverage this system to spin all of our gallery images in unison. 

![rotation](rotation.png)

In our gallery, we want to rotate our images as if they were dangling from a ceiling fan. In order to achieve this, we will rotate the main  `#rotator-node`  backwards on its X axis by 90 degrees ( -&pi;/2 in radians) and then use the Z axis to spin all of the `.gallery-item` images. Additionally, we need to rotate each `.gallery-item` forwards by 90 degrees in order for our images to be visible. 

_Note: Since DOM elements are flat, they become invisible when they are rotated perpendicular to the screen. Note how we didn't quite rotate our `#rotator-node` node above by a full 90 degrees ( only -&pi;/2.1 or 85.7 degrees) just yet so it would be visible in this example._ 

<span class="cta">
[Up next: Setting images &raquo;](./setting-images.html)
</span>
