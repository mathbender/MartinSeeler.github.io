---
date: 2016-01-27 20:29:20
description: Detecting loop in a singly linked list
title: The Tortoise and The Hare

---

### The problem

**Problem 1**: You have a singly linked list where any element contains the address of the next element. If you know there is a loop in the linked list, how do you detect it?

**Problem 2**: For any finite set `$S$` and any function `$f : S \mapsto S$`, if a single `$x_0$` is given, the sequence
<div>`$$x_0, x_1 = f(x_0), x_2 = f(x_1), \cdots, x_{i+1} = f(x_i), \cdots$$`</div>
is bound to repeat by pigeonhole principle. How do you find out the cycle, i.e. where the sequence starts to loop and how long is the cycle?

In case of problem 2, there would be two values `$i \neq j$` such that `$x_i = x_j$`. From that point on, the sequence would continue to repeat, as `$x_{i+1} = f(x_i) = f(x_j) = x_{j+1}$` and so on. There, the cycle detection is finding the minimal `$i, j$`, given the `$f, x_0$`.

Also note that, in both cases, if we iterate over the linked list/sequence, we can go on to infinity, as a loop exists.

### Properties of loops

Before diving in to solve the problem, let us inspect some of the features of the loops.

1. There is a minimal integer `$N$` where the looping starts;
2. There is a period of the loop, which we will call `$M$`. `$M$` is the period means that  
	* For any `$i > N$`, `$x_i = x_{i+M}$`	
	* There are no integer `$m<M$` such that for all `$i > N$`, `$x_{i} = x_{i+m}$`
3. The sequence goes on to infinity (only because it loops back on itself.)
4. If the sequence repeats with period `$M$`, it repeats after `$kM$` terms too, where `$k$` is any natural number. It's because `$x_{i} = x_{i+M} = x_{i+2M} = \cdots = x_{i+kM}$` by induction.

Each of these properties will help us find the existing cycle in the linked list or the sequence.

### Tortoise and the hare

We know that there exists an `$N$`, for any $i > N$, `$x_1 = x_{i + kN}$`. Generalizing that, we can say when `$i = kN$` for a large enough `$i$`, we have `$x_{i} = x_{2i}$`. This is the main idea behind the tortoise and hare algorithm.

![Visualization - tortoise and hare algorithm](https://upload.wikimedia.org/wikipedia/commons/5/5f/Tortoise_and_hare_algorithm.svg "Tortoise and Hare algorithm")
