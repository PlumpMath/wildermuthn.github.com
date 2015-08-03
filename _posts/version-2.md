---
layout: post
title: "A small case for Clojurescript"
description: ""
category:
tags: []
---
{% include JB/setup %}

The best way to demonstrate why Clojurescript is a better language than Javascript is by example. Let's start by comparing the most basic kind of syntax: the `switch` statement.

## Small Cases

```js
switch (shape.type) {
        case 'triangle':
                return shape.base * shape.height * .5;
        case 'rectange':
                return shape.length * shape.width;
        case default:
                return 0;
```

Clojurescript's version of switch, `case`, although compact, doesn't offer a compelling difference:

```clj
(case (:type shape)
    :triangle  (* (:base shape) (:height shape) (/ 1 2))
    :rectangle (* (:length shape) (:width shape))
    :else 0)
```

## Smarter Cases

Switches are hard to add and remove cases from, and the strange syntax often leads to bugs. An smarter alternative to `switch` is to use an object of keyed functions. ES6 recommended.

```js
var getArea = shape => {
  var calcArea = {
    'triange': s => s.base * s.height * .5
    'rectangle': s => s.length * s.width;
    'default': s => 0;
  };
  var calcArea = CalcArea[shape.type] || CalcArea['default'];
  return calcArea[shape.type](shape);
}
```

This examples decouples function dispatch from function defition, allowing for simpler debugging and extension.. But it's a little verbose, even with ES6. In JS, we often face the choice between clarity and conciseness. Clojurescript goes beyond ES6, granting us both through runtime polymorphism.

```clj
(defmulti area :type)

(defmethod area :triangle [{:keys [base height]}]
  (* base height .5))

(defmethod area :rectangle [{:keys [length width]}]
  (* length width))

(defmethod area :default [_] 0)

(area shape)
```
Two things stand out: multimethods and self-documenting destructuring and a pipeline of functions using `->`. This 'threading' macro takes data and passes it through each function, transforming that data step-by-step, function-by-function, until finally returning the fully processed result. This not only decouples data from function, but function from function.

## Bonus Case

Separating out the steps of the data transformation creates room for powerful modifications. For example, replacing the pricing function with a custom function is simple:

```clj
(defn pricer [{:keys [type price] :as order}
              {:keys [default] :as rates}
              custom-pricer]
  (-> rates
      (get type)
      (or default)
      (custom-pricer price))) ;; vs. (* price)

(defn half-off [discount-rate price]
    (* .5 discount-rate price))

(pricer order rates half-off)
```

Clojurescript isn't like Coffeescript. It isn't like JSX. It isn't like Dart or Typescript, or any other language that has tried to fix Javascript. Clojurescript is Clojure, but in your browser -- a pragmatic functional programming language that also happens to be a Lisp.

## Up Next

Clojurescript as the ultimate Flux framework.

<div style= "display:none">
## Blown-Mind Cases

If the type needs to be calculated by the product SKU, as pulled from a REST endpoint? And the result needs to be sent to the state machine instead of being returned directly? And we need to log our results to the console half-way through the process, right before making a second REST call for a prices from the server? Oh, and we should check prices only when the shopping-cart says it is ready for checkout?

```clj

(defn pricer [{:keys [sku price] :as order}  ;; vs. type
              {:keys [default] :as rates}
              custom-pricer
              state-machine]
  (go (-> rates
          (get (<! (request-type sku))     ;; Asynchronous as if synchronous
          (or default)
          (log :red :bold)                 ;; Log partial result in color
          ((<! (server-pricer)) price))    ;; More Async!
          (partial >! state-machine)))))   ;; Notify the SM

(go (while true                            ;; Infinite loop, but only locally
      (-> (<! shopping-cart)               ;; Parks as if threaded
          checking-out?
          (if (pricer order rates server-pricer state-machine)))))

```
</div>

