---
layout: post
title: "Let Gone Wild"
description: ""
category:
tags: []
---
{% include JB/setup %}

Clojure gives me the tools to write in a functional style, but I've found myself unconsciously using `let` and writing imperatively. Why? Probably because it is so easy:

```clj
(defn postage-cost [dimensions weight shipping-days]
  (let [[height width] dimensions
        big? (and (< height 10) (< width 10))
        express? (> 2 shipping-days)
        multiple (cond
                    (and express? big?) 5
                    express? 3
                    big? 2
                    :else 1)]]
    (* multiple weight)))
```

My imperative solution comes from an imperative problem-solving mindset:

1. Grab the height and width from the dimensions.
2. Check height and width to see if the package is big.
3. Check the shipment days to see if we need express shipment.
4. Based on the former calculations, determine the multiplier.
5. Multiply the weight by the multiplier.

There's nothing especially wrong with such logical step-by-step thinking, but I've written a function that is useless except for a very narrow range of situations. When you tell a computer exactly what to do and exactly how to do it, you end up with a brittle program.

### A Functional Solution

So how should I approach this according to Functional Programming principles? What are those principles? There's no definitive list, but I like these:

* Immutable data structures
* Bottom-up programming with discrete functions

### Data

First, put your data where it belongs: in immutable structures.

```clj
(def pricing
  {:express 3
   :big 2
   :regular 1})

(def sizing
  {:big-height 10
   :big-width 10})

(def speed
  {:express 2})
```

### Bottom-up functions

> "It is better to have 100 functions operate on one data structure than 10 functions on 10 data structures." — Alan Perlis

> "Garbage in, garbage out."

Every program basically transforms one set of data (input) into another set of data (output). While thinking imperatively causes me to jump to solution first, a functional approach causes me to look at data first. A simple way to structure this though process is to write down the raw data you expect a program to receive, and then (after putting aside the strong urge to solve the problem!), writing down the raw data you expect the program to produce:

```clj
;;; In
{:width 10
 :height 20
 :weight 4
 :days-to-ship 2}

;;; Some functions do their thing

;;; Out
{:width 10
 :height 20
 :weight 4
 :days-to-ship 2
 :express? true
 :big? true
 :multiplier 5
 :price 20}
```

Here's where bottom-up comes into practice. Identify the missing data, then build the functions that derive that data. Additionally, try to return the same kind of data that comes in (this will be very useful for composing functions).

* Add :price

```clj
(defn add-price
  [{:keys [weight multiplier] :as package}]  ;; destructuring
  (->> (* weight multiplier)
       (assoc package :price)))              ;; returns a copy of `package`
```

* Add :multiplier

```clj
(defn add-multiplier
  [{:keys [express big regular]}
   {:keys [express? big?] :as package}]
  (->> (cond
         (and express? big? (+ express big)
              express? express
              big? big
              :else regular))
       (assoc package :multiplier)))
```

* Add :big?

```clj
(defn add-big
  [{:keys [big-height big-width]}
   {:keys [height width] :as package}]
  (->> (and (< big-height height)
            (< big-width width))
       (assoc package :big?)))
```

* Add :express?

```clj
(defn add-express
  [{:keys [express]}
   {:keys [days-to-ship] :as package}]
  (->> (< days-to-ship express)
       (assoc package :express?)))
```

* Combine functions

```clj
(def add-props
  (map
    partial
    [add-big add-express add-multiplier]        ;; first arg to partial
    [sizing speed pricing]))                    ;; second arg to partial

(def add-shipping (apply comp add-price add-props))
```

Here's where I jump for joy and declare my love for Functional Programming! The use of `map`, `partial`, and `comp` is a sign that something has gone right.

## Value and Cost

> "Move fast and break things"

The FP code is **five** times longer than the imperative solution. That's a huge cost. What kind of code could warrant that kind of effort? I'd propose the following:

1. **Readable**: because small functions can be understood
2. **Reusable**: because discrete functions can be reused
3. **Extensible**: because pure functions can be composed

It's easy to write imperative code. It happens to me daily. But if I've written anything worth remembering, perhaps it is this simple: don't `let` it happen to you too!

