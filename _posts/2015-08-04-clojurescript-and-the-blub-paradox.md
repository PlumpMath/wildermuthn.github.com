---
layout: post
title: "ClojureScript and the Blub Paradox"
description: "ClojureScript as what JavaScript should be"
category:
tags: []
---
{% include JB/setup %}

> I actually really like JavaScript overall (lang + community + tooling, etc). But I always think of the language itself as a three legged puppy. The poor thing was disadvantaged since birth, but darn does it try hard.

> &mdash; Paradoxquine, Clojurian

JavaScript is amazing. No other language can claim its level of ubiquity, running on browsers, servers, desktops, phones, and microcontrollers. Everyone seems to know and use JavaScript. Even so, many of us have experienced the dark side of JavaScript. We've been victims of its "bad parts", and I fear that I've spent more time avoiding problems than solving them.

Maybe that's because JavaScript is a <a href="http://www.paulgraham.com/avg.html">Blub</a> &mdash; a mediocre language that puts fundamental limits on problem-solving. The community, while loving the reach of JavaScript, seems to have reached the same consensus: JavaScript must be fixed. Thus: Coffescript, TypeScript, Dart, ES6/7, BabelJS, and all the other attempts to give JavaScript a fourth leg to stand upon.

But what if we don't need to fix JavaScript? What if, instead, we can supersede JavaScript with something better &mdash; a programming language designed to achieve a vision instead of mending a mistake?

With ClojureScript (Clojure compiled to JavaScript), that's exactly what we get: a dynamic, functional, pragmatic language that can not only improve our software, but more importantly, can improve our thinking.

## A Concrete Example of the Blub Paradox

When you're stuck with JavaScript in the browser, you not only write programs in JavaScript code, you also solve problems with a JavaScript mindset, a mindset limited by the language itself.

That makes the important advantages of ClojureScript hard to perceive, as the essential difference belongs in new patterns of problem solving rather than new syntax and grammar. These new thought patterns can only really be seen from the inside-out, by entertaining them in our own minds. We can only do that by actually using ClojureScript to solve a real problem, and afterwards, examining our thought processes.

So let's pick a problem and solve it with both JS and CLJS. Picking one out of the hat gives us, inevitably, FizzBuzz:

>Write a program that prints the numbers from 1 to 100. But for multiples of three print `Fizz` instead of the number and for the multiples of five print `Buzz`. For numbers which are multiples of both three and five print `FizzBuzz`.


```js
// JS: FizzBuzz

for (var i=1; i <= 20; i++)
{
    if (i % 15 == 0)
        console.log("FizzBuzz");
    else if (i % 3 == 0)
        console.log("Fizz");
    else if (i % 5 == 0)
        console.log("Buzz");
    else
        console.log(i);
}
```

```clj
;; ClojureScript: FizzBuzz

(doseq [n (range 1 101)]
  (println
    (condp = 0
      (mod n 15) "FizzBuzz"
      (mod n 3) "Fizz"
      (mod n 5) "Buzz"
      n)))
```

## Code Comparison as a Red-Herring

The JS and CLJS solutions don't differ dramatically in terms of logic, and we could spend a lot of time debating the merits of brevity, clarity, `if` `else` verses `condp`, a discussion that Rich Hickey, BDFL of Clojure, sums up in his talk <a href="http://www.infoq.com/presentations/Simple-Made-Easy">Simple Made Easy</a>. But for our purposes, the forms of our language matter less than the kind of thoughts that such forms encourage.

Let's look at the code again, but this time going through the most likely pattern of thoughts that generate the solutions.

## JavaScript Problem-Solving Pattern

1. Given a series of numbers, use `for` to iterate.
2. Given the need for remainders, use `%` in each iteration.
3. Given conditions for the remainders, use `switch`.
4. Given JavaScript, never ever use `switch`.
5. Given conditions, use `if` `else if` `else`.

## ClojureScript Problem-Solving Pattern

1. Given the requirement to print lines, focus on side-effects.
2. Given side-effects using a sequence, use `doseq` and `range`
3. Given a pattern of conditions, use `case`, `cond`, or `condp`.
4. Given a pattern, identify abstractions.
5. Given a pattern of checking for remainders of zero,<BR> use `condp`, `=`, `0`, and `mod`.


## Damage Control vs. Problem Solving

> 4\. Given JavaScript, never ever use `switch`.<BR>
> vs.<BR>
> 4\. Given a pattern, identify abstractions.

Just at the step where the most important analytical effort occurs, JavaScript's deficiencies abort logical thought with axiomatic warnings: `switch` statements should be avoided due to `break` bugs. The JavaScript mindset suddenly shifts from problem-solving to damage-control, avoiding the language's "ugly parts". We compound this problem by doing what smart people do: we absorb this knowledge and begin to skip steps 3 and 4 entirely, and in doing so, skip *thought patterns* for building better abstractions.

ClojureScript does the opposite. Instead of seeking to avoid the danger of using *one* bug-prone function, it provides *three* types of switch statements, each suited to different kinds of control flow. These options not only allow one to engage in high-level forms of abstraction, but actually encourage this kind of thinking. Moreover, the entire pattern of thought begins at a much higher level, seeking to recognize patterns even before fully understanding the solution.

## The Best FizzBuzz Solution Possible, Ever

Having spent more time than any human being should allocate to FizzBuzz, and doing so with the mindset encouraged by ClojureScript, I find that `condp` still doesn't fit exactly with the pattern of our solution.  The functionality I'm looking for is, unsurprisingly, called pattern matching.

Pattern matching typically comes built into a language or not, as it relies on special syntax to be simple and clear. ClojureScript, like JavaScript, doesn't come with pattern matching. This would be a problem, except that ClojureScript comes with something better: macros.

```clj
(doseq [n (range 1 101)]
  (println
    (match [(mod n 3) (mod n 5)]    ;; The pattern of remainders: n/3 and n/5
      [0 0] "FizzBuzz"              ;; If both remainders are 0
      [0 _] "Fizz"                  ;; If the first remainder is 0
      [_ 0] "Buzz"                  ;; If the second remainder is 0
      :else n)))
```

## Powerful Code, Powerful Thinking

Some languages run close to the "metal" &mdash; running insanely fast due to a language's close mapping to how computers actually operate. For certain discrete tasks that require maximum performance, such languages are vital.

Other languages take the opposite approach, running close to the "human metal" &mdash; our minds. The closer that a language can express (or in the case of macros, be modified to express) human thoughts, the closer we get to a program that we can understand natively. We no longer need to change our thoughts to fit the language. Instead, we change our language to fit the thoughts.

>LISP is worth learning for a different reason — the profound enlightenment experience you will have when you finally get it. That experience will make you a better programmer for the rest of your days, even if you never actually use LISP itself a lot.

> Eric Steven Raymond, <a href="http://www.catb.org/esr/faqs/hacker-howto.html">How to Become a Hacker</a>

This year is the year we are able to use Lisp all day every day, everywhere and anywhere. And among the many people we have to thank for that, we shouldn't forget the little browser-engine that could: JavaScript.

If you don't know much about ClojureScript, check out what <a href="https://www.reddit.com/r/ClojureScript/top/?sort=top&t=all">the community is up to</a>. From the core contributors to the newest beginner, I've found the Clojure culture to be exceptionally thoughtful, generous, and crazy-smart. I hope you find the same!

<div style="display: none;">
In this small example, I see the kernal of the essential difference between ClojureScript and JavaScript: **clarity** in syntax and structure, and **power** in logic and functionality.

## Clarity

The easiest thing to spot is the visual difference. Unless you have an irrational fear of parens, the clarity of Clojure is unmistakable. The data speaks for itself:

JavaScript uses 217 characters, 14 lines, 9 keywords or operators:

`for` `var` `if` `else` `%` `=` `<=` `++` `===`

ClojureScript uses 154 characters, 6 lines, and 5 keywords:

`dotimes` `condp` `=` `mod` `println`

While getting used to prefix notation can take a period of adjustment, the simplified syntax of ClojureScript makes for dramatic gains in readability. But clarity comes from more than fewer characters and smarter naming.

## The Blub of If-Else

The blub paradox, an idea of YCombinator's Paul Graham, claims that users of a weak programming language will not be able to understand why more powerful programming languages are more powerful. Worse, they can't imagine programming in a different (and better) way. Graham might be overly pessimistic, and his argument is certainly quite abstract. But with with JavaScript vs. ClojureScript, we can make it concrete:

What would it be like to program without `if` and `else`?

In JavaScript, nothing is more prevalent than nested chains of If-Else-If. The alternatives are worse: `switch` will `break` not only your program, but your spirit. But what else is there? The Javscript ecosystem hasn't offered up anything else. Maybe that all there is? Maybe there's no better way?

ClojureScript offers a simple solution: fix `switch` to use smart conditions. Clojurscript has `case`, `cond`, and most poweful of all, `condp`, where conditions must satisfy a predicate function. But ClojureScript doesn't stop with better conditionals. As part of the larger Clojure ecosystem, it embraces everything from types, to protocols, to dynamic polymorphism, to pattern matching:

```clj
(doseq [n (range 1 101)]
  (match [(mod n 3) (mod n 5)]
    [0 0] (println "FizzBuzz")
    [0 _] (println "Fizz")
    [_ 0] (println "Buzz")
    [_ _] (println n)))
```

## Power

The `match` function is not a function at all, it is a macro. A macro is a special kind of function. It doesn't return values like other functions. Instead, it returns code, code that is run in the macros place. In other words, you write code that writes code. You write programs that write programs. Who needs ES6 and Babel, when ClojureScript can compile itself?

Power comes from doing more with less while paradoxically increasing the simplicity of a program. While being called "simple" might be an insult in some realms, a good program should be simple: simple to write, simple to debug, simple to maintain, simple to extend.

ClojureScript is a powerful and simple language. It isn't like Coffeescript. It isn't like Typescript. It isn't a fix-up for JavaScript. Clojure wasn't written for the browser, but unless something dramatic happens, it will certainly be best known for having turned the browser into real platform for serious software engineering.

ClojureScript is a powerful and simple language. It isn't like Coffeescript. It isn't like Typescript. It isn't a fix-up for JavaScript. ClojureScript is Clojure, but in your browser -- a pragmatic functional programming language that also happens to a Lisp.

</div>

