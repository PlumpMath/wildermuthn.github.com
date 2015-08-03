---
layout: post
title: "Clojurescript and the Blub Paradox"
description: "Clojurescript as an objectively superior language than Javscript"
category:
tags: []
---
{% include JB/setup %}

>Blub is good enough for him, because he thinks in Blub.

>&mdash; Paul Graham (<a href="http://www.paulgraham.com/avg.html">Beating the Averages</a>)

Javascript is a Blub &mdash; a mediocre programming language of objectively less power than others. Yet Javascript has one great advantage: it is ubiquitous, running in browsers, servers, smartphones, and even microcontrollers. But what if you could replace Javascript with a more powerful language such as Lisp? What would you get?

You'd get Clojurescript, a lisp that runs just about anywhere on the planet, and also happens to <a href="http://swannodette.github.io/2015/07/29/clojurescript-17/">self-compile</a>.

## A Concrete Example of the Blub Paradox

When you're stuck with Javascript in the browser, you not only write programs in Javascript code, you also solve problems with a Javascript mindset, a mindset limited by the language itself.

That makes the superiority of Clojurescript hard to perceive, as the essential difference belongs in new patterns of problem solving rather than new syntax and grammar. These new thought patterns can only really be seen from the inside-out, by entertaining them in our own minds. We can only do that by actually using Clojurescript to solve a real problem, and afterwards, examining our thought processes.

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
;; Clojurescript: FizzBuzz

(doseq [n (range 1 101)]
  (println
    (condp = 0
      (mod n 15) "FizzBuzz"
      (mod n 3) "Fizz"
      (mod n 5) "Buzz"
      n)))
```

## The Blub Programmer Shrugs

The JS and CLJS solutions don't differ dramatically in terms of logic, but CLJS has managed to sove the problem without using binary conditionals: using `condp` instead of `if` `else`. While that might not seem so important, it reveals a . . .

>Sorry to interrupt, but so what? Sure, using `if` `else` might take about 30 seconds longer to write (unless you're smart and are using code snippets). But using `condp` takes about 3 minutes longer to understand (and thus an exponentially longer time to maintain, expand, and/or test). It seems like you've confused brevity with clarity. I could have used a `switch` statement with expressions for cases, but that would only repeat what appears to be Clojurescript's mistake &mdash; creating complexity where a simple solution suffices.

>&mdash; Advanced Javascript Ninja

## The Shrug of Misidentification

Differences in code caused the Blub Programmer to fixate on what programmers tend to do all day long: write code, test code, maintain code, expand code, debug code, and read a lot of code written by other people. The anxiety experienced in reading unintelligible code isn't unwarranted. But this anxiety over code is a red-herring. It isn't the code that is important, but rather the mindset and patterns of problem-solving that our programming language encourages.

Let's look at the code again, but this time going through the most likely pattern of thoughts that generated the solution.

## Javascript Problem-Solving Pattern

1. Given a series of numbers, use `for` to iterate.
2. Given the need for remainders, use `%` in each iteration.
3. Given conditions for the remainders, use `switch`.
4. Given Javascript, never ever use `switch`.
5. Given conditions, use `if` `else if` `else`.

## Clojurescript Problem-Solving Pattern

1. Given the requirement to print lines, focus on side-effects.
2. Given side-effects using a sequence, use `doseq` and `range`
3. Given a pattern of conditions, use `case`, `cond`, or `condp`.
4. Given a pattern, identify abstractions.
5. Given a pattern of checking for remainders of zero,<BR> use `condp`, `=`, `0`, and `mod`.


## Damage Control vs. Problem Solving

> 4\. Given Javascript, never ever use `switch`.<BR>
> vs.<BR>
> 4\. Given a pattern, identify abstractions.

Just at the step where the most important analytical effort occurs, Javascript's deficiencies abort logical thought with axiomatic warnings: `switch` statements should be avoided due to `break` bugs. The Javascript mindset suddenly shifts from problem-solving to damage-control, avoiding the language's "ugly parts". Over time, steps 3 and 4 merge and disappear, and along with it a chance to see and build better abstractions.

Clojurescript does the opposite. Instead of seeking to avoid the danger of using *one* bug-prone function, it provides *three* types of switch statements, each suited to different kinds of control flow. These options not only allow one to engage in high-level forms of abstraction, but actually encourage this kind of thinking. Moreover, the entire pattern of thought begins at a much higher level, seeking to recognize patterns even before fully understanding the solution.

## The Best FizzBuzz Solution Possible, Ever

Having spent more time than any human being should allocate to FizzBuzz, and doing so with the mindset encouraged by Clojurescript, I find that `condp` still doesn't fit exactly with the pattern of our solution.  The functionality I'm looking for is, unsurprisingly, called pattern matching.

Pattern matching typically comes built into a language or not, as it relies on special syntax to be simple and clear. Clojurescript, like Javascript, doesn't come with pattern matching. This would be a problem, except that Clojurescript comes with something better: macros.


```clj
(doseq [n (range 1 101)]
  (println
    (match [(mod n 3) (mod n 5)]
      [0 0] "FizzBuzz"
      [0 _] "Fizz"
      [_ 0] "Buzz"
      :else n)))
```

Javascript's macro-less <a href="https://github.com/bramstein/funcy">implementation</a>, while likely capable of being improved upon, shows what happens when your language is stuck with the syntax its creators gave it.

```js
for (n = 1; n <= 100; n++) {
  console.log(
    fun(
      [[0, 0], function() { return 'FizzBuzz'; }],
      [[0, _], function() { return 'Fizz'; }],
      [[_, 0], function() { return 'Buzz'; }],
      [_, function() { return n; }]
    )([n % 3, n % 5])
  );
}
```

## Inferior Language, Inferior Thinking

Without the ability to create new forms of syntax using macros, Javascript has to jump through hoops that few will be willing to follow. In the final analysis, the language that forces you to jump through hoops, or remember which hoops not to jump through, is a language that is draining your mental energy and creativity, focusing your attention on trivialities rather than the good stuff.

There's a reason that people embrace Lisp in an almost religious way &mdash; solving problems (the good stuff) is a hell of a lot of fun!


<div style="display: none;">
In this small example, I see the kernal of the essential difference between Clojurescript and Javascript: **clarity** in syntax and structure, and **power** in logic and functionality.

## Clarity

The easiest thing to spot is the visual difference. Unless you have an irrational fear of parens, the clarity of Clojure is unmistakable. The data speaks for itself:

Javascript uses 217 characters, 14 lines, 9 keywords or operators:

`for` `var` `if` `else` `%` `=` `<=` `++` `===`

Clojurescript uses 154 characters, 6 lines, and 5 keywords:

`dotimes` `condp` `=` `mod` `println`

While getting used to prefix notation can take a period of adjustment, the simplified syntax of Clojurescript makes for dramatic gains in readability. But clarity comes from more than fewer characters and smarter naming.

## The Blub of If-Else

The blub paradox, an idea of YCombinator's Paul Graham, claims that users of a weak programming language will not be able to understand why more powerful programming languages are more powerful. Worse, they can't imagine programming in a different (and better) way. Graham might be overly pessimistic, and his argument is certainly quite abstract. But with with Javascript vs. Clojurescript, we can make it concrete:

What would it be like to program without `if` and `else`?

In Javascript, nothing is more prevalent than nested chains of If-Else-If. The alternatives are worse: `switch` will `break` not only your program, but your spirit. But what else is there? The Javscript ecosystem hasn't offered up anything else. Maybe that all there is? Maybe there's no better way?

Clojurescript offers a simple solution: fix `switch` to use smart conditions. Clojurscript has `case`, `cond`, and most poweful of all, `condp`, where conditions must satisfy a predicate function. But Clojurescript doesn't stop with better conditionals. As part of the larger Clojure ecosystem, it embraces everything from types, to protocols, to dynamic polymorphism, to pattern matching:

```clj
(doseq [n (range 1 101)]
  (match [(mod n 3) (mod n 5)]
    [0 0] (println "FizzBuzz")
    [0 _] (println "Fizz")
    [_ 0] (println "Buzz")
    [_ _] (println n)))
```

## Power

The `match` function is not a function at all, it is a macro. A macro is a special kind of function. It doesn't return values like other functions. Instead, it returns code, code that is run in the macros place. In other words, you write code that writes code. You write programs that write programs. Who needs ES6 and Babel, when Clojurescript can compile itself?

Power comes from doing more with less while paradoxically increasing the simplicity of a program. While being called "simple" might be an insult in some realms, a good program should be simple: simple to write, simple to debug, simple to maintain, simple to extend.

Clojurescript is a powerful and simple language. It isn't like Coffeescript. It isn't like Typescript. It isn't a fix-up for Javascript. Clojure wasn't written for the browser, but unless something dramatic happens, it will certainly be best known for having turned the browser into real platform for serious software engineering.

Clojurescript is a powerful and simple language. It isn't like Coffeescript. It isn't like Typescript. It isn't a fix-up for Javascript. Clojurescript is Clojure, but in your browser -- a pragmatic functional programming language that also happens to a Lisp.

</div>

