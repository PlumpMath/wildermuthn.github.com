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
switch (order.type) {
        case 'vip':
                return order.price * .50;
        case 'preferred':
                return order.price * .75
        case default:
                return order.price;
```

Clojurescript's version of switch, `case`, although compact, doesn't offer a compelling difference:

```clj
(case (:type order)
  :vip         (* (:price order) .50)
  :preferred   (* (:price order) .75)
               (* (:price order)))
```

## Smarter Cases

It's likely that both our pricing function and our percentage discounts will be dynamic, generated according to circumstance. In that case, our small cases fail. We need an abstraction that allows us to separate data from functionality.

An smarter alternative to `switch` is to simply separate data from functionality.

```js
/* Data */
var rates = {
  'vip': .50,
  'preferred': .75,
  'default': 1
};

/* Function */
var pricer = function(o, r) {
  var rate = r[o.type] || r['default'];
  return o.price * rate;
}

/* Calculation */
var cost = pricer(order, rates);
```

This example separates data from functionality, allowing `rates`, `pricer`, and `order` to be debugged, tested, and modified in isolation. But it's a little verbose. In JS, we often face the choice between clarity and conciseness.

Clojurescript offers us both:

```clj
;; Data
(def rates {:vip .5
            :preferred .75
            :default 1})

;; Function
(defn pricer [{:keys [type price] :as order}  ;; Destructuring
              {:keys [default] :as rates}]
  (-> rates                                   ;; Threading
      (get type)
      (or default)
      (* price)))

;; Calculation
(pricer order rates)

```

Two things stand out: self-documenting destructuring and a pipeline of functions using `->`. This 'threading' macro takes data and passes it through each function, transforming that data step-by-step, function-by-function, until finally returning the fully processed result. This not only decouples data from function, but function from function.

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

