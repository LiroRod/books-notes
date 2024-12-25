# Pairs of reals

The set of all pairs of reals is written `R^2`. So it contains pairs like ([-2.3, 4.7]).

Similarly, the set `R^3` contains triples of reals such as [8, 7.3, -9]. In a computer program, we implement a pair of reals using and object or struct with two field.

**Suppose someone presents you with a pair of numbers ([x, y]) or one of the corresponding data structures.** 
Your first question should be: what is the pair intended to represent? It might be:

- a point in 2D space
- a complex number
- a rational number (if the second coordinate isn't zero)
- an interval of the real line

The intended meaning affects what operations we can do on the pairs.

For example, if ([x, y]) is an interval of the real line, it's a set. So we can write things like `z ∈ (x,y)` meaning `z` is in the interval ([x, y]). That notation would be meaningless if ([x, y]) is a 2D point or a number.

If the pairs are numbers, we can add them, but the result depends on what they are representing.
So [(x, y) + (a, b)] is [(x + a,y + b)] for 2D points and complex numbers. But it is [(xb + ya,yb)] for rationals.

Suppose we want to multiply [(x, y) x (a, b)] and get another pair as output.
There's no obvious way to do this for 2D points. For rationals, the formula would be [(xa, yb)], but for complex numbers it's [(xa - yb,ya + xb)].

Stop back and work out that last one for the non-ECE students, using more familiar notation. 

[(x + yi)(a + bi)] is [xa + yai + xbi + byi^2]. But [i^2] = - 1.

So this reduces to [(xa - yb) + (ya + xb)i].

>> Oddly enough, you can also multiply two intervals of the real line. This carves out a rectangular region of the 2D plane, with sides determined by the two intervals.
