---
layout: post
title: "The Ultimate Flux Framework"
description: ""
category:
tags: []
---
{% include JB/setup %}

Problems with Angular

* Data Mutation in Controllers
* Data Mutation in Services
* Uncoordinated data mutation
* Server-side rendering
* Resort to deep cloning = SLOW

React Solutions
* Replace angular directives
* Replace two-day data binding with one-way
* Renders to string on server
* Removing data mutation/watcherse = FAST

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




