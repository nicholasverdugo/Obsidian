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
- Let f be a continuous function, [a,b] with f(a) < 0 and f(b) > 0
- Then f must have at least one zero in (a,b)
![[Pasted image 20210830152429.png]]
- The bisection method will find one zero that has a sign-change
- Each iterate x^(k) will be an approximation to α, the zero of f which the method converges to.
	- Idea: Keep splitting the interval in half.
		- Choose the sub interval for which f has different signs at the endpoints as the new interval.
		- It must contain a zero.
		- Repeat.
	- Initializing the method: choose a < b
		- set a^(0)=a, b^(0)=b
		- for k=0,1,2,...
		- increment k
		- if f(x^(k-1))=0
			- then α = x^(k-1)
			- `method would terminate here`

## Assignments to-do
1. #📉MAD4401 
	1. Due on [[2021-09-04]]
	2. HW1