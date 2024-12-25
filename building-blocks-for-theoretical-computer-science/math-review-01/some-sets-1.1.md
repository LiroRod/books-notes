# Some Sets

### Positive Numbers
Notice that a number is positive if it is greater than zero, so zero is not positive. If you wish to exclude negative values, but include zero, you must use the term `"non-negative"`.
>> In this book, natural numbers are defined to include zero.

### Real Numbers
Notice also that the real numbers contain the integers, so a `"real"` number is not required to have a decimal part. And, finally, infinity is not a number in standard mathematics.

#### Real Number includes:
The real numbers contain all the rationals, plus irrational numbers such as `√2` and `π`. (Also `e`, but we don't use e much in this end of computer science.)
>> We'll assume that `√x` returns (only) the positive square root of `x`.

### Rational Numbers
Rational numbers are a set of fractions `(p/q)` where q can't be zero and we consider two fractions to be the same number if they are the same when you reduce them to lowest terms.

### Complex Numbers
The complex numbers are numbers of the form `a + bi`, where `a` and `b` are real numbers and `i` = `√-1`

### Notations
If we want to say that the variable `x` is a real, we write `x ∈ R`. Similarly `y  ̸∈ Z` means that `y` is not an integer. In calculus, you don't have to think much about the type of your variables, because they are largely reals.
>> In this book we handle variables of several different types, so it's important to state the types explicitly.


It is sometimes useful to select the real numbers in some limited range, called an `interval` of the real line. 
We use the following notation:

- closed interval: [[a,b]] is the set of real numbers from `a` to `b`, including `a` and `b`.
- open interval: ([a,b]) is the set of real number from `a` to `b`, not including `a` and `b`.
- half-open intervals: [[a,b]) and ([a, b]] include only one of the two end-points.

