---
layout: default
title: Timelines
---

Timelines make complex animations easy by allowing you to manipulate several behaviors over a given period of time. 

## Structure

To create a new timeline, use the `.timelines()` method that can be chained to the framework component. This method accepts an object with the timeline names and definitions containing selectors and behaviors. 

    FamousFramework.component('user:timeline-example', {
      behaviors: {},
      events: {}.
      states: {}.
      tree: '<node id="foo"></node>'
    }).timelines({
        'animation1': {
            #foo {
            // Animate behaviors here
            }
        }
    });



Above, the `'animation1'` timeline targets the `#foo` node, which it will animate by accessing `#foo`'s behavior functions. The `.timelines()` method can accept multiple timelines, each with any number of selectors and behavior animations contained within objects. 

Since timelines create "tween" animations between the values provided, only behaviors that take numerical values can be used. In other words, behaviors such as `'content'` or `'style'` cannot be used in a timeline.

  
## Animating a behavior

 We create animations by transitioning a component's behavior values smoothly over time. While transitions can be created through events, it can become burdensome when trying to coordinate multiple animations. This is where Timelines excel. Timelines allow you to specify any number of value changes between 0 - 100% of the timeline's duration.  

 Each behavior listed (under the selectors in the timeline definition) accepts an object whose keys represent percentages relating to the timeline's own duration. These can be thought of as the "key frames" in an animation. We then assign these duration percentages objects containing a new behavior value (under the `value` key) and an optional easing curve (under the `curve` key). 

    FamousFramework.component('user:timeline-example', {
        behaviors: {
            '#foo': {
                'size':[100,100],
                'style':{
                    'background-color':'blue'
                }
            }
        },
        events: {}.
        states: {}.
        tree: '<node id="foo"></node>'
    }).timelines({
        'animation1': {
            #foo {
                'rotation-z': {
                    '0%':   { value: 0, curve: 'linear' },
                    '50%':  { value: Math.PI * 2, curve: 'linear'},
                    '100%':  { value: 0, curve: 'linear' }
                }
            }
        }
    });

Starting at rotation of zero, we rotate `#foo`'s `'rotation-z'` behavior PI * 2 radians (one full rotation) by the halfway mark (50%) and then reverse its rotation back to 0 using a linear curve. Behind the scenes, the timeline creates a smooth transition between these values and gives us a spinning animation.


## Adding an event

Once a timeline is defined, event functions can access it by dependency injecting the `$timelines` object. This object exposes a `.get()` method for locating an animation and a `.start()` method for triggering it.

    events: {
        '#foo': {
            'click': function($timelines){
                $timelines.get('animation1').start({ duration: 2000 });
            }
        }
    }

Note how we pass the `$timeline.start()` method an object with a duration in milliseconds. This duration will be used to trigger the behavior changes assigned to percentage values in the timeline. So, at 1 second in or halfway through the animation (50% of 2000ms), the `#foo` node will finish rotating in a complete circle.   
 
  
## Animating multiple behaviors

We can define multiple timelines, each with multiple selectors and behaviors. Below is a slightly more complex example with 2 animations. 

        .timelines({
            'animation1': {
                // Animation 1 code
            },
            'animation2': {
                '#foo': {
                    'scale': {
                        '0%':   { value: [1,1] },
                        '50%':  { value: [0,0], curve: 'easeInOut'},
                        '100%':  { value: [1,1], curve: 'inOutBounce' }
                    },
                    'position': {
                        '0%':   { value: [0, 0, 0], curve: 'linear' },
                        '50%':  { value: [-500, -500, 0], curve: 'easeOutBounce'},
                        '100%':  { value: [0, 0, 0], curve: 'easeOutBounce' }
                    },
                    'rotation-z': {
                        '0%':   { value: 0 },
                        '50%':  { value: Math.PI * 2},
                        '100%':  { value: 0, curve: 'outBounce' }
                    }
                },
                '#bar':{
                    'scale': {
                        '0%': [0.5,0.5],
                        '50%':[1,1]
                    }

                    // additional behaviors can go here
                
                }
            }
        });

These timelines can be targeted individually using `$timelines.get()`, or added to a timeline queue.

## Creating an animation queue

The `$timelines` object can queue muliple animations using the `.queue()` method.

    $timelines.queue([
        [ 'animation1', { duration: 2000 }],
        [ 'animation2', { duration: 2000 }]
    ]).startQueue();

The `.queue()` method accepts multiple animations passed in as an array of arrays. 

## Callbacks

The `$timelines.queue()` method can also be used to trigger callback functions at the end of animations. 

    events: {
        '#foo': {
            'click': function($timelines){
                // $timelines.get('animation1').start({ duration: 1500 });
                $timelines.queue([
                    [ 'animation1', { duration: 2000 }, ()=> console.log('Animation 1 finished')],
                    [ 'animation2', { duration: 2000 }, ()=> console.log('Animation 2 finished')]
                ], function(){
                    console.log("All of the animation are finished");
                }).startQueue();
            }
        }
    }

Add a callback as the third item in the timeline selector array passed to the `.queue()` method. After the animation in the array is complete, the callback will fire before calling on the next animation. 
