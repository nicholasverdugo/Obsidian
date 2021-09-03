# [[2021-09-03]]
### Class Notes:
- Let α solve f(x)=0
	- If f'(α)=0 then Newton's method converges only linearly
	- If f'(α)=0, f''(α)=0, ... , f^(m)(α)=0 then Newton's method converges linearly with the rate C=1-ym
	- |x^(k+1)-α| <= C|x^k-α|
- terminating newton iterations
	- in the bisection method we could determine how many iterations k ensure the error is less than a tolerance E
	- |e^k|=|x^k-α| < E
	- Of course, we don't know the error
	- Strategy 1: nased on increment
		- |x^k-x^(k-1)| < E
	- Strategy 2: we want f(x)=0
		- Check |f(x^k)|
		- Terminate based on the residual r^k=f(x^k)
		- |f(x^k)| < E
	- if f'(α) ~~ 1 the residual works well
	- 
## Assignments to-do
1. #📉MAD4401
	1. Due on [[2021-09-04]]
	2. hw1