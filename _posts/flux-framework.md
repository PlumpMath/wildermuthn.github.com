---
layout: post
title: "Porting a Production Application from Angular to ClojureScript"
description: ""
category:
tags: []
---
{% include JB/setup %}

In 2012, Angular felt like a gift from the gods: data and DOM fused together, if one part changed, the other part did too. No more event listeners. No more manipulating the DOM. Just pure power.

Armed with that power, last year I began work on an ambitious project: a video management and analytics app, more akin to a desktop program than a dynamic web-page. And in one month, I declared the project complete, feeling brilliant and amazing for having met all my deadlines and requirements.

Then the bug reports started pouring in. And for the next month, I played wack-a-mole.

During this period of intense pressure, with each day revealing new problems, I began some deep reconsideration of my beloved framework. I noticed a pattern in the problems:  data and DOM fused together, when anything changed, everything else did too. No more control. Just pure power, gone rogue.


## Angular in Production

Keeping track of data mutation is easy when you don't have much data. But in building a Google Anlytics clone, my small team of three str



Problems with Angular

* Data Mutation in Controllers
* Data Mutation in Services
* Uncoordinated data mutation
* Resort to deep cloning = SLOW

React Solutions
* Replace angular directives
* Replace two-day data binding with one-way
* Removing data mutation/watchers = FAST

Flux solutions
* Replace service methods with actions
* Replace 2-way data-binding in UI with actions

React problems
* JSX testing, tooling
* Gulp/Grunt integration
* Verbose
* Module loading server components vs client components
* Webpack hot reloading doesn't work very well

Flux Problems
* Data mutation = deep cloning = SLOW
* Store data, being mutable, could still be corrupted in controllers
"If you're using Flux, you should start writing your stores using immutable-js. Take a look at the full API."
* Dispatcher not suited to complex UI actions, especially async. Hence flux frameworks galore.

Semi-Clojurescript Solutions
* ES6 with Babel
* Mori's immutable data
* js-CSPs Go routines
* FP programming libraries

Clojurescript solutions
* Mutable vs Immutable
* Semi-functional vs wholly fp
* Babel vs Macros
* Modules vs Namespacing
* Grunt/Gulp + 1000s vs Google JS Compiler
* Devtools vs REPL and Hot Code reloading




