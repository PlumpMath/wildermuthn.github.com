---
layout: post
title: "The Ultimate Flux Framework"
description: ""
category:
tags: []
---
{% include JB/setup %}

# Looking For

* Collaborate for needs (background in Consulting, currently serving news-room people) I know how to listen and communicate about underlying needs vs proposed solutions

* Developing requires great communication with back-end people. I've learned how to do that effectively being remote, and coming in for visits.

* Next level? Can't get further than what I'm doing with JS. I'd propose using Electon for a desktop application.

* Graphs using charting libraries, but would use D3. I'm most aware of performance difficulties and ensuring the server returns the right kind of data that wont kill our app.

* Good ML libraries for Java === Clojure, for server-side rendering before it hits user's screen.

* UI Engineers want to use. "Real-time rules and conditions" That means a DSL. That means Lisp.

* Extensible? Provide an SDK for building their own modules. Make the App a true black-box. No hacking away at core CLJS functionality.


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

2-3 years
service get rebuilt
improve always

AB testing platform

Backend is solid

Customers all are internal

MVP approach
- specific cases
- 60% for all teams (marketting, but also data scientists)
- Extensible, build out analytics?

UI in place with google webtoolkit.

Freedom and Responsibility.

LCD

New framework --> For power users

Schedular and Planner for AB testing

Can't run parallel, have to be sequential.

Search for conflicts.

Scheduled tests with targetting information, filtering.
Proposed schedules.

ABlaze

Meta-data about the test.

# Questions
- How technically inclined are the users
- Different kind of data? Different kind of data sources?
- Build out analytics?





