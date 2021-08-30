# [[2021-08-30|Today]]
### Class Notes: #📉MAD4401 
- Computing the zeroes of a real-valued function f
	- Find x such that f(x)=0
	- a.k.a. root finding
- More generally, f:R->R (function with a vector value)
	- find x ele. R such that f(x)=0
- if f is a polynomial of deg>4, there is no explicit formula to find its roots
- if f is not a polynomial, finding its zeroes can be more difficult
- Iterative methods produce a sequence of approximations x^1, x^2, x^3, ...
	- these approximate a zero x of the function f
- There are 4 methods we are going to use in this class to solve single non-linear equations:
	1. Bisection (ID)
	2. Newton's method (ID and systems)
	3. Secant method (ID, with extensions to systems)
	4. Fixed-point method (ID and systems)
-> there is another method, `Newton-Horner` that is specific to polynomial root finding

#### Method 1 : Bisection
- Let f be a continuous function, [a,b] with f(a) < 0 and f(b) < 0

## Assignments to-do
1. #📉MAD4401 
	1. Due on [[2021-09-04]]
	2. HW1