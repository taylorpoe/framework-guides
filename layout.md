---
layout: default
title: Layout
---


This guide will teach you the basics of layout, i.e. the sizing and positioning of elements in 3D space. All layout is controlled within a component's behaviors and is directly affected by its location in the tree. 

## Size

The most basic way to size a node is through the `'size'` behavior. `'size'` lets us set the size of a node in pixels for width, height, and depth (X, Y, and Z respectively) using the following syntax:
    
    FamousFramework.component('example', { 
        tree: '<node id="foo"></node>',
        behaviors: {
            '#foo': { 
              // note: While DOM elements are flat, 
              // WebGL meshes are 3D so they need a Z value 
                'size': [100,100,100]
            }
        }
    });

We can also use the `'size'` behavior to set true size, which is the size of the content.
        
        FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
                '#foo': {
                    'size': [true,true],
                    content: `this node will
                              be sized to
                              fit this text`
                }
            }
        });

    
When a node's size is _`undefined`_ it inherits its size from its parent. 
   
    FamousFramework.component('example', { 
        tree: '<node id="#foo"></node>',
        behaviors: {
            '#foo': {
                'size': [undefined,100],
                content:`this node will
                        be 100px tall and 
                        the width of the 
                        screen wide`
            }
        }
    });

    FamousFramework.deploy('example', 'HEAD', 'body');

Since the X and Z values are undefined above, they will inherit their width and depth from the root context or, in this case, the `'body'` tag where it is deployed. Here, X will fill the width of the screen. If embedded in an `<iframe>` or `<div>`, then the component would fill up the full width of that element. 

## Sizing Modes

Famous gives us three size modes: absolute, relative and render sizing. These size modes let developers fine tune the size of elements for X, Y, and Z values independently. 


## Absolute size

Absolute size sets the pixel values of a node. The `'size'` behavior accepts pixel values directly, but you can also use the `'size-absolute'`, `size-absolute-x`, `size-absolute-y`, `size-absolute-z` behaviors to explicitly set absolute size. These may come in handy when mixing different size modes.

### Relative Size: Proportional & Differential

Relative size is calculated based off the parent's size. When a size isn't set for a node, then the size defaults to relative size and it inherits the full size of the parent. Relative size is calculated based on the following equation:

     node size = parent size * proportional + differential 

Proportional sizes are provided as percentages listed as decimal fractions. The following snippet sizes a node to half the size of its parent.  

    FamousFramework.component('example', { 
        tree: '<node id="foo"></node>',
        behaviors: {
           '#foo': {
                'size-proportional': [0.5,0.5]
            }
        }
    });

    FamousFramework.deploy('example', 'HEAD', 'body');

Note how `[0.5,0.5]` translates to 50% of parent's width (X) and height (Y). Unlike absolute size, relative size updates when the parent's size changes. Since `#foo` is deployed to the `'body'`, the node will be half the size of the browser window and it will remain that size if we resize the window.

Differential size can be used in addition to or in place of proportional size. Differential size is simply a pixel value added to the size of the parent.

    FamousFramework.component('example', { 
        tree: '<node id="foo"></node>',
        behaviors: {
           '#foo': {
                'size-proportional': [0.5,0.5]
                'size-differential': [100,250]
            }
        }
    });


The final size of the node above will be 100px greater than half the parent's width and 250 pixels greater than half the parent's height. Similar to `'size-absolute'`, the following behaviors are provided for independently setting X, Y, and Z values: `'size-proportional-x'`, 
`'size-proportional-y'`, `'size-proportional-x'`, `'size-differential-x'`, `'size-differential-y'`, `'size-differential-z'`.


### Render size: True size

As mentioned previous, we specify the `'size'` behavior's values as _`true`_ in order to get the size of the rendered content. Additionally, we can use the `'size-true'` behavior:

    FamousFramework.component('example', { 
        tree: '<node id="foo"></node>',
        behaviors: {
           '#foo': {
                'size-true': [true,true],
                'size-true-z': true
            }
        }
    });

The following behaviors are also provided to explicitly set an  X, Y, or Z value: `'size-true-x'`, `'size-true-y'`, `'size-true-z'`


## Scale

We recommend using the `'scale'` behavior for animating changes in size. Unlike the `'size'` behavior, scale is hardware accelerated and responds a lot better to changes, especially in DOM intensive applications. Note that `'origin'` affects how a node is scaled (see Origin section below).


        FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
               '#foo': {
                    'size':[500,500]
                    'scale': [0.5,0.5] 
                }
            }
        });

Scale values are multiplied against the current size of the node. The `#foo`
node above would be 250px wide and 250px tall. 

## Positioning

All elements are positioned relative to the parent node in the tree. Positioning begins from the top left corner of the parent context and moves towards the bottom right as you increase the X and Y values. Increasing the Z value moves an element forward.

## Position

The `'position'` behavior  "translates" a node along its X, Y and Z axes by a certain pixel value. 
   
        FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
               '#foo': {
                    'position': [100,250]
                }
            }
        });

The behavior above moves the node 100 pixels to the right (X) and 250 pixel down (Y) from the top left of its parent. 

## Align

The `'align'` behavior positions an element relative to its parent's size or "context". Values provided to align are multiplied against the size of the parent and used to position an element. Similar to proportional sizing the values are provided as percentages listed as decimal fractions.
    
        FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
               '#foo': {
                    'align': [0.5,0.5]
                }
            }
        });

The node above is positioned to the center of its parent (50% of parent's height and width). Unlike `'position'`, when the size of the parent changes the `'align'` will update the node's position automatically. 

## Mount Point

The `'mount-point'` behavior defines a point (or anchor) within the node's bounding box where a linear translation is applied. By default the mount point is set to the top left of the node. `'mount-point'` accepts percentages as decimal fractions that get multiplied against the current size of the node.
    
        FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
               '#foo': {
                    'align': [0.5,0.5],
                    'mount-point': [0.5,0.5]
                }
            }
        });

Setting both mount point and align to `[0.5,0.5]` places the center of the node at the center of its parent.

<span class="sidenote"><strong>Still don't understand?</strong> Take a Post-it note and place it flat (sticky side up) on your desk. Now, take a single finger and use it to move the Post-it to the other side of your desk. Think of your finger as the mount point. Not setting the mount-point would be the equivalent of placing your finger at the top left of the Post-it before moving it. Setting mount point to [0.5,0.5] would be the equivalent of placing your finger in the middle of the Post-it and moving it along.</span>   


## Origin 

The `'origin'` behavior is similar to mount point, but it sets the anchor point for rotations and scale. The origin defines the point --on the node-- where it should rotate around or scale from.
    
        FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
               '#foo': {
                    'origin': [0.5,0.5],
                    'scale': [0.5,0.5] // node will be half the size
                                       // of its parent and scaled
                                       // from its center
                }
            }
        });

Like `'mount-point'` and `'align'`, the `'origin'` behavior accepts X, Y and Z percentage values as decimal fractions. Origin is set in relation to the node's own bounding box (size).

For rotating a node around its center point like a pinwheel, you must set a node's origin to [0.5, 0.5] before incrementing the node's Z rotation value. Otherwise by default, rotations (and scale) occur from the top left corner of the node.

## Rotation

The `'rotation'` behavior specifies the rotation of a node in radians. A rotation will be applied to the node wherever the origin point is set.
    
    FamousFramework.component('example', { 
            tree: '<node id="foo"></node>',
            behaviors: {
               '#foo': {
                    'rotation': [0,0,Math.PI/2],
                    'origin': [0.5,0.5]
                }
            }
        });

Above, we rotate `#foo` a quarter turn to the right on its Z axis and from its center (see origin) like a pinwheel. Rotations can either be specified in Euler angles (above) or quaternions. Constantly switching between Euler angles and quaternions on a frame-by-frame basis results into computationally expensive conversions and should be avoided. 

Note that Quaternions have some advantages when it comes to gimbal lock and smooth interpolation, but they rely on advanced math that even experienced developers can find difficult and confusing.


