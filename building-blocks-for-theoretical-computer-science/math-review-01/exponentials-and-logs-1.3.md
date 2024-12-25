# Exponentials and logs

Suppose that `b` is any real number. We all know how to take integer powers of `b` for example: [b^n] is `b` multiplied by itself `n` times.

It's not so clear how to precisely define `b^x`, but we've all got faith that it works (e.g. our calculators produce values for it) and it's a smooth function that grows really fast as the input gets bigger and agrees with the integer definition on integer inputs.

  Here are some special cases to know:

  - [b^0] is one for any [b]
  - [b^0.5] is [√b]
  - [b^-1] is [1/b]

  And some handy rules for manipulating exponents:
    [b^x b^y = b ^ x+y]
    [a^x b^y = (ab)^x]
    [(b^x)^y = b^xy]
    [b^(x^y) != (b^x)^y]

Suppose that [b > 1]. Then we can invert the function [y = b^x], to get the function [x = logb y] ("logarithm of y to the base b"). Logarithms appear in computer science as the running times of particularly fast algorithms.

>>  Notice that the log function takes only positive numbers as inputs.

In this book, [log x] with no explicit base always means [log2 x] because analysis of computer algorithms makes such heavy use of base-2 numbers and powers of 2.


Useful facts about logarithms include:
    [b^logb(x) = x]
    [logb(xy) = logb x + logb y]
    [logb(x^y) = y logb x]
    [logb x != loga x logb a]

>> Most importantly, notice that the multiplier to change bases is a constant, i.e. doesn't depend on x. So it just shifts the curse up and down without really changing its shape.
