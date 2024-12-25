# Some Handy functions

Suppose that `k` is any positive integer. Then `k` factorial, written [k!], is the product of all the positive integers up to and including `k`. That is:

[k! = 1 . 2 . 4 . ... . (k - 1) . k]

For example, [5! = 1 . 2 . 3 . 4 . 5 = 120.] 
>> [0!] is defined to be [1]

Suppose that we have a set `S` containing `n` objects, all different. There are `n!` *permutations* of these objects,
i.e. `n!` ways to arrange the objects into a particular order.

>> There are [n!]/[k!(n-k)!] ways to choose `k` (unordered) elements from `S`. The quantity [n!]/[k!(n-k)!] is frequently abbreviated as (n k), pronounced "n choose k".For

The max function returns the largest of its inputs. For example, suppose we define [f(x) = max(x2, 7)] then: [f(3) = 9] but [f(2) = 7].
>> The min function is similar.

The floor and ceiling functions are heavily used in computer science, though not in many areas other of science and engineering.

Both functions take a real number x as input and return an integer near [x]. The floor function returns the largest integer no bigger than [x]. In other words, it converts [x] to an integer, rounding down.

This is written [⌊x⌋]. If the input to floor is already an integer it is returned  unchanged. Notice that floor roundes downward even for negative numbers, so:

[
⌊3.75⌋ = 3
⌊3⌋ = 3
⌊−3.75⌋=−4
]

The ceiling function, written [⌈x⌉], is similar, but rounds upwards. For example:
[
⌈3.75⌉ = 4
⌈3⌉ = 3
⌈−3.75⌉=−3
]

Most programming languages have these two functions, plus a function that rounds to the nearest integer and one that "truncates" i.e. rounds towards zero. Round is often used in statistical programs. Truncate isn't used much in theoretical analyses.For
