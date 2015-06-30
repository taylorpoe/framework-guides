---
layout: default
title: Control Flow
---

The Famous Framework currently supports three basic control-flow operations: `$if`, `$repeat`, and `$yield`. Unlike other template-based frameworks, the Famous Framework doesn't support programming these kinds of operations inside the structural declaration of your component (i.e. in the tree). Instead, control flow must be implemented as [behaviors](behaviors.html).

## $if

The `$if` control-flow behavior is a special behavior that will add/remove selected components from the scene graph based on a boolean return value. Here's a simple example, in which an element is removed from the scene when it is clicked:

    FamousFramework.component('jane-doe:control-flow-if', {
        behaviors: {
            '#el': {
                // Elements in the tree that match the `#el` selector will
                // be added/removed from the scene depending on whether the
                // result of this function is true/false respectively
                '$if': function(toggle) {
                    return toggle;
                }
            }
        },
        events: {
            '#el': {
                'click': function($state) {
                    $state.set('toggle', false);
                }
            }
        },
        states: { toggle: true },
        tree: `<node id="el"><p>Now you see me...</p></node>`,
    });

## $repeat

The `$repeat` control-flow behavior will repeat the selected components a certain number of times, where the number of times reflects the `.length` of the array returned from the function. For example, the following example repeats a `node` three times:

       FamousFramework.component('jane-doe:repeat', {
            tree: `<node class="el"></node>`,
            behaviors: {
                '.el': {
                    '$repeat': function() {
                        return [
                            {position:[0,0],content:'Hello'},
                            {position:[50,50],content:'Howdy'},
                            {position:[100,100],content:'Ahoy'}
                        ];
                    }
                }
            }
        });

If objects are given as the array elements returned by the `$repeat` behavior, the properties of those objects will be sent as event messages to each repeated component as it is initialized.

Injecting `$index` into any behavior function allows you to access the index of an item in the `$repeat` array. Additionally, the `$index` parameter notifies the framework to call that behavior function for each item in the `$repeat` array. 

     FamousFramework.component('jane-doe:repeat', {
            tree: `<node class="el"></node>`,
            states: {
                colors: ['red', 'yellow', 'blue', 'green']
            }
            behaviors: {
                '.el': {
                    '$repeat': function(colors) {
                        return colors;
                    },
                    style: function($index, colors){
                       return {
                        'background-color': colors[$index]
                       }
                    }
                }
            }
        });

Above, the `colors` array (in the states) is injected into the `$repeat` function and a new node is created for each item in the array. Note how we use `$index` to set a different background color for each node we create.

## $yield

The `$yield` control-flow behavior allows a component to define the conditions under which a parent can insert content into it. In other words, when a `$yield` action occurs, control of some region within a component's tree is "yielded" to the parent.

        // jane-doe/example/example.js
        // This scene allows content to "punch through"
       FamousFramework.component('jane-doe:example', {
            tree: `
                <node id="main"></node>
                <node id="sidebar"></node>
            `,
            behaviors: {
                '#main': {
                    '$yield': '#main-content'
                },
                '#sidebar': {
                    '$yield': '#sidebar-content'
                }
            }
        });

        // someone/else/else.js is the parent component
        // This scene uses of jane-doe:example's $yield behavior
       FamousFramework.component('someone:else', {
            tree: `
                <jane-doe:example>
                    <node id="main-content"></node>
                    <node id="sidebar-content"></node>
                </jane-doe:example>
            `
        })

In the example above, we introduce one component, `jane-doe:example`, which makes use of the `$yield` behavior. It has a specific rule for when another (parent) component tries to define content as its child.

For `jane-doe:example`, the injected content will only be allowed if the injected elements have `id="main-content"` or `id="sidebar-content"`. Moreover, it specifically designates the elements within its own tree that the surrogate content will be placed. (`#main-content` will be inserted into `#main`, and `#sidebar-content` will be inserted into `#sidebar`.)

`$yield` is one of the most fundamental control-flow operations in the Famous Framework, because it makes component nesting, layouts, and default/overridable content possible. And although most components will never need to use `$yield` behaviors directly, almost all will indirectly make use of it -- any time they declare even a simple nested tree:

    <node>
        <!-- `node` uses 'yield' to allow `other-thing` to be injected here -->
        <other-thing></other-thing>
    </node>

One of the most commonly used pre-built components in the Famous Framework, `<famous:core:node>` (or just `<node>`) exists mainly as an empty container that content can be injected into. 

## A word of warning about performance

Control flow operations should be used sparingly, when possible, because they make direct modifications to the scene graph structure any time they run. (Modifying the scene graph is a potentially expensive operation.)

If hiding an element is the goal, for example, it may make sense to toggle a node's `opacity` instead of removing it from the scene entirely via the much-heavier `$if` behavior.
