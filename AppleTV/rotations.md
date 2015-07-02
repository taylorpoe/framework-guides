---
layout: default
title: Rotations
---


Let's complete our image gallery by adding the neat spin animation to the images. Instead of adding the spin animation to a timer like the real Apple TV screensaver, we will instead trigger the spin animation when you click on an image. 

## Listen for clicks

Begin by adding a selector for `.gallery-item` right below `$lifecycle` and giving it a `click` event. Similar to `'post-load'`, we'll assign `'click'` a function and inject a `$state` object into it so we can modify our component's state. If you're following along, make sure your `events` object looks like the code below:


            events: { 
            '$lifecycle': {
                'post-load': function($state, $famousNode){
                    var id = $famousNode.addComponent({
                        onUpdate: function(time) {
                            for(var i=0; i < $state.get('srcs').length; i++ ){
                                var currentZ = $state.get(['positionZ',i])
                        // if image is out of screen move it back to bottom
                                if(currentZ < -$state.get('contextSize')){
                                    currentZ = $state.get('contextSize')/1.5+100;
                                }
                                $state.set(['positionZ',i], currentZ-1);  
                            }
                            $famousNode.requestUpdateOnNextTick(id);
                        }
                    });
                    $famousNode.requestUpdateOnNextTick(id);
                }
            },
            '.gallery-item': {
                'click': function($state){
                      
                    console.log('I clicked an image!')

                }
            }
        }

If you copied the code above, it should now output a message to the console when you click on an image. Let's replace this message with the rotation animation.

## Transition animations

Previously, we used the `$state` object to simply get and set values. However, we can use this object to create "tween" animations as well. The `$state.set()` method accepts a third options hash parameter that can specify a duration and easing curve for smoothly transitioning between old and new values. The `duration` determines how long a transition should take and the easing curve defines how the frames should be distributed when creating the "tween" animation. In addition to setting a `duration` and `curve`, we can also chain one or many `.thenSet()` methods on to `$state.set()` for back to back transitions. 

Let's see all of this in action: 

    
     $state.set('rotationValue', $state.get('rotationValue') - Math.PI/2, {
                     duration: 1000,
                     curve: 'easeIn',
                 }).thenSet('rotationValue', $state.get('rotationValue')-(Math.PI*2), {
                     duration: 2000,
                     curve: 'easeOut',
             });

Above, we perform two back to back rotation animations, each with different easing curves. This will give us a unique, full rotation animation on a click event.

## Wiggle the scene

In order to give our rotating animation a bit more emphasis, we will also move the whole spin event backwards and forwards. Add the code below to the body of the `click` event above. 

    $state.set('rootZ', -250, {
        duration: 1000,
        curve: 'easeOut'
    }).thenSet('rootZ', 0, {
        duration:200, 
        curve: 'easeInOut'
    });

Next, add a `rootZ` value to your states and a `position-z` behavior function to the `'#rotator-node'` selector passing in and returning `rootZ`:
    


    behaviors: {

        //other behavior selectors not shown

        '#rotator-node':{
        
            // other behaviors not shown

            'position-z':function(rootZ){
                return rootZ
            }
        }
    },

    states {
        
        // other states not shown

        rootZ: 0
    }

Note that the coordinates are not flipped for the `#rotator-node`, just its children. Moving the Z position of `#rotator-node` will have the effect of moving it forwards and back. 


## Cleaning it up

We are almost finished! Just a couple more things to make your gallery complete:

  - Remove the styles on the `#rotator-node`
  - Modify `#rotator-node`'s X rotation value so it rotates a full quarter turn (-&pi;/2 instead of -&pi;/2.1)
  - Add your favorite background color to the CSS of index.html (The true apple TV screensaver uses black)

When you are finished your full component code should look like the code below:

    FamousFramework.component('my-name:apple-tv', {
            behaviors: {
                '#root': {
                    'style': {
                        'perspective': '1000px',
                    }
                },
                '#rotator-node': {
                    'position-z':function(rootZ){
                       return rootZ
                    },
                    'size': function(contextSize){ 
                        return [contextSize, contextSize]
                    },         
                    'align': [0.5,0.5],          
                    'mount-point':[0.5,0.5],
                    'origin':[0.5,0.5],     
                    'rotation': function(rotationValue){ 
                        return [-Math.PI/2, 0, rotationValue] 
                    }
                },
                '.gallery-item':{
                    'size': [100,100] 
                    ,
                    '$repeat':function(srcs){
                        return srcs   
                    },
                    'position-x': function($index, contextSize){ 
                        return Math.random()* contextSize
                    },
                    'position-y':function($index, contextSize){ 
                        return Math.random()* contextSize
                    },
                    'position-z': function($index, positionZ){ 
                        return positionZ[$index] 
                    },
                    'rotation': [Math.PI/2,0,0], 
                    'content': function($index, srcs){
                         return  `<img src="${ srcs[$index] }" style="height:100px;width:100px"/>`
                    }
                }
            },          
            events: { 
                '$lifecycle': {
                    'post-load': function($state, $famousNode){
                        var id = $famousNode.addComponent({
                            onUpdate: function(time) {
                                for(var i=0; i < $state.get('srcs').length; i++ ){
                                    var currentZ = $state.get(['positionZ',i])
                            // if image is out of screen move it back to bottom
                                    if(currentZ < -$state.get('contextSize')){
                                        currentZ = $state.get('contextSize')/1.5+100;
                                    }
                                    $state.set(['positionZ',i], currentZ-1);  
                                }
                                $famousNode.requestUpdateOnNextTick(id);
                            }
                        });
                        $famousNode.requestUpdateOnNextTick(id);
                    }
                },
                '.gallery-item': {
                    'click': function($state){
                        $state.set('rotationValue', $state.get('rotationValue') - Math.PI/2, {
                            duration: 1000,
                            curve: 'easeIn',
                        }).thenSet('rotationValue', $state.get('rotationValue')-(Math.PI*2), {
                            duration: 2000,
                            curve: 'easeOut',
                        });

                        $state.set('rootZ', -500, {
                            duration: 1000,
                            curve: 'easeOut'
                        }).thenSet('rootZ', 0, {
                            duration:2000, 
                            curve: 'easeInOut'
                        });

                    }
                }
            },              
            states: {
                rotationValue: 0,    
                srcs: imageData,    
                contextSize: contextSize,
                positionZ: randomCoordinates(imageData),
                rootZ: 0
            },              
            tree: 'apple-tv.html'  
    }).config({
         includes: [
             'galleryData.js',
             'apple-tv.css'
         ]
    });
        
<span class="cta">
[Up next: Finish &raquo;](./finish.html)
</span>
