---
layout: default
title: Structuring an app
---

In this section, we will teach you how to plan out an application and structure your code accordingly.

## What are we building?
 
Here's a list of everything we want our gallery to do:

  - Display multiple images at random positions
  - Slowly move them to the top of the screen repeatedly
  - Rotate images in unison around a central point

<!--Starting from the top, we will need to create elements for each of our images. Since these elements will be identical besides the image displayed, we'll make use of the `$repeat` control flow statement in our code. 

For moving the images to the top, we need to store the vertical position of our image nodes in the module's [state](#). That way we can slowly increment the values in regular intervals and our behaviors will respond. When the nodes get to the top of the screen, we can move them back to the bottom.

Finally, since we want to rotate all of our images in unison, we will attach all of them to a single shared node. This is where the scene graph will come in to play. 
-->
## Building the Tree

Now that we have an idea of what we're building, we can build out a [Tree](../tree.html) to match. 

<span class="sidenote"> The Tree will serve as a representation of the [Famous Scene graph](http://famous.org/learn/scene-graph.html) used to structure our project. Building out the Tree will give us foundation where we can attach our behaviors and store relationships of elements in an ordered hierarchy.</span> 

Open up `apple-tv.html` and  add the following code to it. Note the XML like syntax for creating our Tree and how we add traditional HTML id's and classes to it.
    
    <node id="root">
        <node id="rotator-node">
            <node class="gallery-item">
            </node>
        </node>
    </node>

In the snippet above, pay attention to how we nest a single `'gallery-item'` node within the main `'rotator-node'`. This makes it so when we move the `'rotator-node'` all `'gallery-item'` nodes will move with it in unison. 

You may be wondering why we only include a single `'gallery-item'` in our tree when we will have multiple images in our gallery. That's because we will repeat this element in our behaviors, not our tree. It's important to note that all control flow logic, like repeating elements and creating dynamic content, is handled in behaviors. 

Let's see how we will repeat `'gallery-item'` in the next section

<span class="cta">
[Up next: Behaviors &raquo;](./behaviors.html)
</span>
