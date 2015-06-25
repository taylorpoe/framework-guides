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

Now that the simple animation is set up above, we need a way to trigger it. We will do this by adding a click event to `#foo` and injecting the `$timelines` object. This object exposes a `.get()` method for locating an animation and a `.start()` method for triggering it.

    events: {
        '#foo': {
            'click': function($timelines){
                $timelines.get('animation1').start({ duration: 2000 });
            }
        }
    }

Note how we pass the `$timeline.start()` method an object with a duration in milliseconds. This duration will be used to trigger the behavior changes assigned to percentage values in the timeline. So, at 1 second in or halfway through the animation (50% of 2000ms), the `#foo` node will finish rotating in a complete circle.   

## Adjusting the axis of rotation

At this point, the animation rotates from the upper left corner of `#foo`. The axis of rotation is defined by a node's <em>origin</em> property. Let's set the rotation point to the center of the `#foo` and also move the `#foo` to the center of the screen using the <em>align</em> property.

    '#foo': {
        'content': '<h1>Timeline Example</h1>',
        'size': [200, 200],
        'style': {
            'background-color': 'red'
        },
        'origin': [0.5, 0.5],
        'align': [0.5, 0.5]
    }

## Creating an animation queue

Now that we have our first animation set up, let's take a look at how to queue up multiple animations. 

    $timelines.queue([
        [ 'animation1', { duration: 2000 }],
        [ 'animation2', { duration: 2000 }]
    ]).startQueue();

As you can see, instead of using `$timelines.get()` to get a single animation we are using `$timelines.queue()` to add multiple animations to a queue. In this way, we can play multiple animations one after another. Let's add this second animation to our timeline.
  
## Animating multiple behaviors

        .timelines({
            'animation1': {
                // Animation 1 code
            },
            'animation2': {
                '#foo': {
                    'scale': {
                        '0%':   { value: [1,1], curve: 'linear' },
                        '50%':  { value: [0,0], curve: 'linear'},
                        '100%':  { value: [1,1], curve: 'linear' }
                    },
                    'position': {
                        '0%':   { value: [0, 0, 0], curve: 'linear' },
                        '50%':  { value: [-500, -500, 0], curve: 'easeOutBounce'},
                        '100%':  { value: [0, 0, 0], curve: 'easeOutBounce' }
                    },
                    'rotation-z': {
                        '0%':   { value: 0, curve: 'linear' },
                        '50%':  { value: Math.PI * 2, curve: 'linear'},
                        '100%':  { value: 0, curve: 'linear' }
                    }
                }
            }
        });

In this animation, we are manipulating multiple behaviors of our `#foo` node. Another feature of animations that can be very useful is the use of <em>callbacks</em>. These can be added to the end of an animations queue.

## Callbacks

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

Using callbacks is a great way to trigger events that rely on the completion of an animation.
