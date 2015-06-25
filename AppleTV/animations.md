---
layout: default
title: Animations
---

Since our behaviors listen to states, we will use events to change these values and animate our scene. 

## Movin' on up

Let's create an update loop to increment the vertical position of our image nodes. As we mentioned previously, we will need to decrement the Z index since our coordinate system is flipped and images are positively positioned off the screen. 

Before we go into the details, add the following code to your events object: 

	     events: { 
        '$lifecycle': {
            'post-load': function($state, $famousNode){
                //add a component with an `onUpdate()` method
                var id = $famousNode.addComponent({
                    onUpdate: function(time) {
                    	//go through all 'positionZ' values
                        for(var i=0; i < $state.get('srcs').length; i++ ){
                            // get current value
                            var currentZ = $state.get(['positionZ',i])
                            // set new decremented value 
                            $state.set(['positionZ',i], currentZ-1);  
                        }
                        //add self to the update queue and create loop
                        $famousNode.requestUpdateOnNextTick(id);
                    }
                });
                //start the loop
                $famousNode.requestUpdateOnNextTick(id);
                }
            }
        }
	    
The `$lifecycle` above gives us access to certain program events that deal with the lifecycle of the scene. More specifically, we use the `post-load` event on `$lifecycle` to trigger an update loop (`.requestUpdateOnNextTick()` ) when the Framework component is loaded.  

In the code above, notice how we dependency-inject two important objects into the `'post-load'` function: `$state` and `$famous`. Let's go over them below:
   
  - `$state` lets us access the state object and modify the values stored within.
  - `$famousNode` lets us access the the "raw" Famous node and attach a component with an `onUpdate` method to create an update loop.

The `.requestUpdateOnNextTick()` call above lets the Famous Engine know that we want to update the component on the next tick of the Engine. The `onUpdate` method (a special property that can be added to all nodes and assigned any function) grabs the Z positions (using the `$state` object), sets a new decremented value, and then requests another update. The result is `onUpdate` being called up to 60 frames per second, producing a smooth animation. 

## Rinse and repeat

Once an image moves past the the top, let's move it back to the bottom of the screen so the animation will repeat over and over again. Add the following control flow statement within the `for` loop in the `onUpdate` method:

    events: { 
        '$lifecycle': {
            'post-load': function($state, $famousNode){
                var id = $famousNode.addComponent({
                    onUpdate: function(time) {
                        for(var i=0; i < $state.get('srcs').length; i++ ){
                            var currentZ = $state.get(['positionZ',i])
                    // if image is out of screen move it back to bottom
                            if(currentZ < -$state.get('contextSize')){
                                currentZ = $state.get('contextSize')+100;
                            }
                            $state.set(['positionZ',i], currentZ-1);  
                        }
                        $famousNode.requestUpdateOnNextTick(id);
                    }
                });
                $famousNode.requestUpdateOnNextTick(id);
            }
        }
    }

Now the images should endlessly animate to the top. 



<span class="cta">
[Up next: Rotations &raquo;](./rotations.html)
</span>
