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
	1. For any `$i \geq N$`, `$x_i = x_{i+M}$`	
	2. There are no integer `$m<M$` such that for all `$i \geq N$`, `$x_{i} = x_{i+m}$`
3. The sequence goes on to infinity (only because it loops back on itself.)
4. If the sequence repeats with period `$M$`, it repeats after `$kM$` terms too, where `$k$` is any natural number. It's because `$x_{i} = x_{i+M} = x_{i+2M} = \cdots = x_{i+kM}$` by induction.

Each of these properties will help us find the existing cycle in the linked list or the sequence.

### Tortoise and the hare

We know that there exists an `$N$`, for any $i \geq N$, `$x_i = x_{i + kM}$`. Generalizing that, we can say when `$i = kM$` for a large enough `$i$`, we have `$x_{i} = x_{2i}$`. This is the main idea behind the tortoise and hare algorithm - we will try to find an index `$i$` such that `$x_{i} = x_{2i}$` and work backword from there.

![Visualization - tortoise and hare algorithm](https://upload.wikimedia.org/wikipedia/commons/5/5f/Tortoise_and_hare_algorithm.svg "Tortoise and Hare algorithm")

We have two pointers, one we will call the _tortoise_ and the other we will call the _hare_. At each move, the tortoise moves one position to the front while the hare moves two position to the front. As such, at each point, we have the tortoise at `$x_i$` and the hare at `$x_{2i}$`.

Now, if, at any moment, if we know hare and tortoise are pointing at the same value, we know that `$x_i = x_{2i}$`, and we now know two particular pieces of information:

1. $i > N$
2. $M | i$

In general, we now know that `$x_{N} = x_{N + i}$`. So, we can find the value of `$N$` by just iterating through the sequence starting from `$x_0$` and noting the first value where `$x_{k} = x_{k+i}$`. From there, we will know `$N = k$`.

Now, we can easily find `$M$` too, because `$M$` is the minimal natural number for which `$x_{N} = x_{N+i}$`. So, we can start iterating from `$x_{N}$` and find the minimal `$i$` such that `$x_{N} = x_{N+i}$`, which would be `$M$`.

And suddenly, we are done! We have all the infos we need, which are the constants `$N, M$`.

### Complexity analysis

We can split the algorithm in three parts and calculate the complexity for each, and then we can find the total complexity by summing the three parts.

For the first part, we found the least `$i$` such that `$x_{i} = x_{2i}$`. For this, we have the following conditions on `$i$`.

1. `$i \geq N$`
2. `$M \mid i$`

If we combine the two conditions, we see that the minimal such `$i$` would be the least multiple of `$M$` greater that `$N$`. By at least one integer from $N$ to $N+M-1$ would be divisible by $M$, so the maximum amount of iteration needed at this step would be $N+M$; and consequently the runtime of this step would be `$O(N+M)$`.

For the second part, we need to find the `$N$` starting from `$x_0$`. Trivially, this step needs `$O(N)$` runtime.

Finally, for the third part, we need to find `$M$` starting from `$x_N$`. This step takes `$M$` iterations and thus this step has complexity `$O(M)$`.

Combining this three steps, we can finally say this algorithm has **complexity `$O(N+M)$`** where `$N$` is the point where the loop starts and `$M$` is the period of the loop.

### The Implementation

```python
def floyd(f, x0):
    # Main phase of algorithm: finding a repetition x_i = x_2i
    # The hare moves twice as quickly as the tortoise and
    # the distance between them increases by 1 at each step.
    # Eventually they will both be inside the cycle and then,
    # at some point, the distance between them will be
    # divisible by the period M.
    tortoise = f(x0) # f(x0) is the element/node next to x0.
    hare = f(f(x0))
    while tortoise != hare:
        tortoise = f(tortoise)
        hare = f(f(hare))
  
    # At this point the tortoise position, i, which is also equal
    # to the distance between hare and tortoise, is divisible by
    # the period M. So hare moving in circle one step at a time, 
    # and tortoise (reset to x0) moving towards the circle, will 
    # intersect at the beginning of the circle. Because the 
    # distance between them is constant at 2i, a multiple of M,
    # they will agree as soon as the tortoise reaches index N.

    # Find the position N of first repetition.    
    N = 0
    tortoise = x0
    while tortoise != hare:
        tortoise = f(tortoise)
        hare = f(hare)   # Hare and tortoise move at same speed
        N += 1
 
    # Find the length of the shortest cycle starting from x_N
    # The hare moves one step at a time while tortoise is still.
    # lam is incremented until M is found.
    M = 1
    hare = f(tortoise)
    while tortoise != hare:
        hare = f(hare)
        M += 1
 
    return M, N
```


******

### Extras

This algorithm is attributed to [Robert W. Floyd](https://en.wikipedia.org/wiki/Robert_W._Floyd) by Donald Knuth, though this was never publised by Floyd in a paper. Probably this was a folk algorithm, not invented by anyone specific. There are several uses of this algorithm other than the obvious use of this to find the cycle length, including the [Rho algorithm](https://en.wikipedia.org/wiki/Pollard%27s_rho_algorithm) to find the prime factorization of a number within approximately $O(N^{\frac 14})$ time.

### Further Reading

1. [Wikipedia - Cycle detection](https://en.wikipedia.org/wiki/Cycle_detection)
2. [Wikipedia - Tortoise and Hare](https://en.wikipedia.org/wiki/Cycle_detection#Tortoise_and_hare)
3. [Floyd's cycle detection algorithm - Siafoo](http://www.siafoo.net/algorithm/10)
4. [Knuth, Donald E.](https://en.wikipedia.org/wiki/Donald_Knuth) (1969), The Art of Computer Programming, vol. II: Seminumerical Algorithms, Addison-Wesley, p. 7, exercises 6 and 7
