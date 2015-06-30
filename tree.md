---
layout: default
title: Tree
---

The `tree` defines the structure of an application’s elements in an ordered hierarchy. Ultimately, it represents the structure of the scene graph that will be created by the Famous Engine. Each component has only one tree, which can be represented using HTML syntax:

	<node>
	    <node></node>
	</node>


<h2 id="node">What is a node?</h2>

 The `<node>` is the framework's core component from which all other components inherit. It is the framework equivalent of a Famous Engine scene graph [node]() represented in an easy to read XML format. We use this syntax so it is more familiar to web developers. 

## Why nodes?

Think of a `<node>` as a container holding all data --sizing, position, DOM Elements, WebGL Meshes, etc.-- that gets rendered to the screen. Instead of sizing positioning DOM elements and WebGl meshes directly, we size and position the _nodes_ that carry our draw data. This lets us control DOM and GL together under a single, easy-to-use coordinate system. 

<h2 id="scene-graph">Scene Graph Hierarchy</h2>

When we add `<node>` components to the tree, we are really just building out a representation of the Famous Engine [scene graph](http://famous.org/learn/scene-graph.html). The tree gives structure to a project and organizes nodes in a logical hierarchy that will interpret the data we attach ( via behaviors ) and pass information down to "child" nodes. 

Every node in your tree has a parent that it inherits data from. When we nest `<node>` elements in the tree, the outer node becomes the parent and its data, including layout, get passed down to the nested child. 

    FamousFramework.component('example',{
      tree: `<node id="parent">
                 <node id="child">
                     <node id="grandchild">
                       <!-- a node here would be a great grandchild node -->
                     </node>
                 </node>
                 <!-- a node here would be a sibling node -->
             </node>
      `
    });

If we size and position the `#parent` or `'#child'` node above, this data will cascade down to the `'#grandchild'` node and anything else nested between it. However changes on the `'#child'` and `'#grandchild'` will not affect the `'#parent'` node or any other sibling nodes.

If there isn't a parent node ( e.g. `'#parent'` above ), then the node will inherit from the "root context", which is either the browser window or the container the project is embedded in.

Above, we use ES6 multi-line strings with the backtick (<code>&#96;</code>) to define our tree (we can also do this by file reference or through a simple string).

## Attaching behaviors

We use CSS-like selectors in our behaviors to target nodes in our tree. 

    FamousFramework.component('example',{
          tree: '<node id="foo" class="bar"></node>',
          behaviors: {
             '#foo': {
                'position':[250,50],
                'size': [100,100]
             },
             '.bar': {
                'style': {
                  'background-color': 'red'
                }
             }
          }
    })

Note how we use two different selectors in the snippet above to target the same node.

Most valid CSS selectors will work with framework, but we recommend using tag names, classes or ids. 

## Inserting actual HTML

HTML content can be added to the tree and interpreted. However, it must be wrapped in a node if you wish to attach behaviors to it. Conversely, events can be attached directly to HTML elements in the tree. 

    FamousFramework.component('examples',{
          behaviors: {
             '#foo': { //this will work input
                'position':[250,150]
             },
             '#bar': {  // this will not work on input
                'position':[1000,100] 
             }
          },
          events: {
            '#bar': { // this will work on input
                'input': function($event){
                    console.log($event)
                }
            }
          },
           tree: `<node id="foo">
                    <input id="bar" type="range"></input>
                  </node>`,
    })
Additionally, the event above will "bubble" up to `'#foo'` where it can also be captured. 

We can also set HTML content through the `content` behavior. Note when adding elements in this manner, the actual element cannot be targeted in events:

    FamousFramework.component('example',{
      behaviors: {
         '#foo': { //this will work input
            'position':[250,150],
            'content': '<input id="bar" type="range"></input>'
         }
      },
      events: {
        '#foo': { // this will work on input
            'input': function($event){
                console.log($event)
            },
        }
        '#bar': {
            'input': function($event){
                console.log('this will never run')
            }
        }
        
      },
       tree: `<node id="foo">
                
              </node>`,
    })

## Dynamic Content

 A Framework component's tree is a pure and simple, logic-less template. All control flow statements (`$repeat`, `$yield`, `$if`) and dynamic data should be added through behaviors. See the [control flow](control-flow.html) section for more info.


