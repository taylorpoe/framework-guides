---
layout: default
title: Hello Framework
---

The [Famous Framework](https://github.com/Famous/framework) provides a structured API for controlling UI elements with the Famous Engine. As a framework, its goals are to bring composability, extensibility, and consistency to UI applications.

## The basics

All Famous Framework projects begin with the following syntax:
 
    FamousFramework.component('module-name',  { /* module definition /* });

The bulk of a project lives within the _module definition_, where we list our behaviors, events, states, and tree as members of an object (see [Core Concepts](core-concepts.html) for an intro to the BEST architectural pattern).

## A simple project

Check out the 'Hello World' example below. When reading through the code, think of the _tree_ as Famous-enhanced HTML and the _behaviors_ as CSS styles on steroids.

    /**
    *  hello-framework.js
    **/
    
    FamousFramework.component('my-name:hello-framework',  {
        behaviors: {
            '#background': {
                'style': {
                    'background': 'linear-gradient(to right, #00B9D7, #9783F2)'
                }
            },
            '#text': {
               'size': [400, 80],
               'align': [0.5, 0.5],
               'mount-point': [0.5, 0.5],
               'style': {
                   'color': 'white',
                   'font-family': 'Lato',
                   'font-size': '60px',
                   'text-align': 'center'
               }
            }
        },
        events: {},
        states: {},
        tree: `
            <node id='background'>
            <node id='text'> 
                <div> Hello Framework! </div>
            </node>
            </node>
        `
    });

Note how we use CSS selectors (`#background`, `#text`) to target behaviors to elements that are declared in the `tree`.

While the _tree_ sets up the structure of our app, the _behaviors_ tell the module how each element should be displayed. The result: a 'Hello Framework!' message centered and styled in front of a CSS gradient background.

## Deploying your component

Deploying a project is as easy as running the following command from the terminal:

  famous deploy

Calling this from the project directory will push your project up to the Famous cloud and return the following lines of code:
    
    Share: https://api-beta.famo.us/codemanager/v1/containers/the-unique-id-of-your-project/share
    Embed:
    <script src="https://assets-beta.famo.us/embed/embed.js"></script>
    <div class="famous-container" data-famous-container-identifier="the-unique-id-of-your-project"></div>

Visit the share link to view your hosted project live on the web or use the embed tags to include your project in an existing website. Note that you will need to size the `<div>` using CSS in order for your project to be visible.  
