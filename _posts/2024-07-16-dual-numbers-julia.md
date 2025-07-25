---
title: "Building a Differentiator Using Dual Numbers In Julia"
date: 2025-01-14 12:00:00 +0000
author: Soham Sud
categories: [machine-learning, misc-maths, projects]
tags: [julia, math, differentiation, dual-numbers]
math: true
---

{% include view-counter.html counter_id="dual-numbers-julia" label="Live Views" initial=1123 %}

## What Are Dual Numbers?

Dual numbers extend the real numbers by introducing a new element, $\epsilon$, with the property:

$$
\epsilon^2 = 0, \quad \text{but} \quad \epsilon \neq 0.
$$

A dual number can be written in the form:

$$
x + \epsilon y,
$$

where $x$ is the "real" part and $y$ is the "dual" part. The key idea is that $\epsilon^2 = 0$, which allows us to elegantly encode derivatives.

## How Do Dual Numbers Enable Differentiation?

Suppose we have a function $f(x)$. When evaluated with a dual number input $x + \epsilon y$, the function returns:

$$
f(x + \epsilon y) = f(x) + \epsilon f'(x) y.
$$

The term $f(x)$ represents the value of the function, and the term $\epsilon f'(x) y$ encodes the derivative. By setting $y = 1$, we extract both $f(x)$ and $f'(x)$ in one evaluation. This is the essence of *forward-mode automatic differentiation*.

## How Does Our Code Work?

Our implementation defines a custom data type for dual numbers, supports arithmetic operations (addition, multiplication, etc.), and overloads common mathematical functions like $\sin(x)$, $\cos(x)$, and $\exp(x)$. These overloaded operations propagate the dual part of a number, enabling us to compute derivatives as follows:

- **Addition:** $(a + \epsilon b) + (c + \epsilon d) = (a + c) + \epsilon (b + d)$
- **Multiplication:** $(a + \epsilon b) \cdot (c + \epsilon d) = ac + \epsilon (ad + bc)$
- **Trigonometric Functions:** $\sin(a + \epsilon b) = \sin(a) + \epsilon b \cos(a)$

By evaluating a function with a dual number $\text{Dual}(x, 1.0)$, the "epsilon part" of the result directly gives the derivative.

### Example Code for Derivative Calculation

Here is how we implemented the dual number differentiator:

```julia
struct Dual
    value::Float64
    epsilon::Float64
end

function derivative(f::Function, x0::Float64)
    f(Dual(x0, 1.0)).epsilon
end

f(x) = x^2 + sin(x)
derivative(f, 2.0)  # Output: 4.0 + cos(2.0)
```

This code outputs the derivative of $f(x)$ at $x = 2.0$ by leveraging the propagation of the dual number's $\epsilon$-part.

## Results from Our Code

Let's verify two examples:

1. $f(x, y) = x^2 + \sin(y)$:  
   At $(x, y) = (2, \pi/4)$, the gradient is:
   $$
   \nabla f(2, \pi/4) = [4, \cos(\pi/4)] = [4, 0.7071].
   $$
2. $g(x, y, z) = e^x + y^4 - \cos(z)$:  
   At $(x, y, z) = (1, 2, \pi/3)$, the gradient is:
   $$
   \nabla g(1, 2, \pi/3) = [e^1, 3 \cdot 2^2, \sin(\pi/3)] = [2.718, 12, 0.866].
   $$

These results demonstrate the power of dual numbers for automatic differentiation.

## Conclusion

Dual numbers provide an elegant way to compute derivatives through forward-mode automatic differentiation. By representing a number as $x + \epsilon y$, where $\epsilon^2 = 0$, we can propagate derivatives through functions using simple arithmetic. This approach is efficient and well-suited for tasks like gradient computation in optimization problems.

If you would like to use a Differentiator using the Dual Numbers method, I have implemented this (for both single and multivariate functions) on my [GitHub page](https://github.com/ssohamsud). 