---
layout: default
title: Imports
---

When making use of other modules, scenes can sometimes get more verbose than we'd like. Take this example:

    FamousFramework.component('foo:bar', {
        behaviors: {
            '#el': {
                'size': [200, 200]
            }
        },
        events: {
            '#el': {
                'famous:events:click': function($state) {
                    $state.set('foo', 1);
                }
            }
        },
        tree: `
            <famous:core:node id="el">
              <robert.smitherson:accordion>
                  <michael.raymond.jones:list-item>
                      <elise.brown:triangle-image />
                  </michael.raymond.jones:list-item>
              </robert.smitherson:accordion>
            </famous:core:node>
        `
    });

Having to refer to all of these dependencies by their full name makes the code more verbose and more difficult to grasp at a glance.

To mitigate this, an `imports` object can be given with the `.config` method call that can be chained to the `FamousFramework.component` invocation. The following example is equivalent to the above:

    FamousFramework.component('foo:bar', {
        behaviors: {
            '#el': {
                'size': [200, 200]
            }
        },
        events: {
            '#el': {
                'click': function($state) {
                    $state.set('foo', 1);
                }
            }
        },
        tree: `
            <node id="el">
              <accordion>
                  <list-item>
                      <triangle-image />
                  </list-item>
              </accordion>
            </node>
        `
    })
    .config({
        imports: {
            'famous:core': ['node'],
            'famous:events': ['click'],
            'robert.smitherson': ['accordion'],
            'michael.raymond.jones': ['list-item'],
            'elise.brown': ['triangle-image']
        }
    });

## Modules imported by default

Several core modules are already imported for you by default:

    {
        'famous:core': [
            'node',
        ],
        'famous:events': [
            'click',
            'dblclick',
            'keydown',
            'keypress',
            'keyup',
            'mousedown',
            'mousemove',
            'mouseout',
            'mouseover',
            'mouseup'
        ]
    }

This explains why you are able to refer to, e.g., `<node>` in your tree without needing to import it via its full module name.