# CSE-3202: Numerical Methods

## Final Examination Question Sets — 2022 & 2023

**Department of Computer Science and Engineering** **Affiliated Engineering Colleges, University of Dhaka**

---

# 📄 2023 Question Set

---

## Question 1

**(a)** Find the real root of the equation `cos x = 3x − 1` correct to five decimal points using **Fixed Point Iteration method**. `[6]`

**(b)** Find the solution to the following system of equations using the **Gauss-Seidel Method** correct to four decimal places: `[8]`

```
6x + 15y + 2z  = 72
 x +  y  + 54z = 110
27x +  6y −  z = 85
```

---

## Question 2

**(a)** Solve the following equations using **Gauss-Jordan Elimination method**: `[6]`

```
 2x₁ +  x₂ − 3x₃ =  11
 4x₁ − 2x₂ + 3x₃ =   8
−2x₁ + 2x₂ −  x₃ =  −6
```

**(b)** Find a root of the equation `x⁶ − x − 1 = 0` using **Secant method** with approximations x₀ = 2 and x₁ = 1. `[6]`

---

## Question 3

**(a)** Find a root of the equation `x³ − 3x − 5 = 0` using the **Newton-Raphson method**, correct up to four decimal places, starting with x₀ = 2. `[6]`

**(b)** Solve the following system of linear equations using **Cholesky's factorization method**: `[8]`

```
2x − 6y + 8z = 24
5x + 4y − 3z =  2
3x +  y + 2z = 16
```

---

## Question 4

**(a)** Evaluate `∫₀¹ e^(−x²) dx` using **Simpson's 1/3** and **Simpson's 3/8 rule**, taking n = 4. `[6]`

**(b)** Use the **Runge-Kutta method of 4th order** for the differential equation: `[8]`

$$\frac{dy}{dx} = \frac{y^2 - x^2}{y^2 + x^2}, \quad y(0) = 1$$

Find y at x = 0.2 and x = 0.4.

---

## Question 5

**(a)** Solve the following system of linear equations using **Cramer's Rule**: `[6]`

```
 5x −  2y + 9z = −7
−2x +   y − 4z =  5
 3x − 10y − 8z =  0
```

**(b)** Using **Taylor series method** with the first five terms in the expansion, find y(0.1) correct to three decimal places, given that: `[8]`

$$\frac{dy}{dx} = e^x - y^2, \quad y(0) = 1$$

---

## Question 6

**(a)** Find y(0.2) for `y' = x − y²`, y(0) = 1, with step length h = 0.1 using **Modified Euler method**. `[7]`

**(b)** Use **Picard's method** up to 3rd approximation to find the value of y when x = 0.25, given that: `[7]`

$$\frac{dy}{dx} = x^2 y - y, \quad y(0) = 1$$

---

## Question 7

**(a)** Use **Euler's method** to compute y(0.9) from the following differential equation: `[6]`

$$\frac{dy}{dx} = x^2, \quad y(0) = 1, \quad h = 0.3$$

**(b)** Use **Picard's method** to solve the following up to 3rd approximation: `[6]`

$$\frac{dy}{dx} = x + y^2, \quad y(0) = 1$$

**(c)** Write down the formula for **Modified Euler's method**. `[2]`

---

---

# 📄 2022 Question Set

---

## Question 1

**(a)** Evaluate the sum `S = √3 + √5 + √7` to 4 significant digits and find its absolute and relative error. `[3]`

**(b)** Find a root, correct to three decimal places and lying between 0 and 0.5, of the equation `4e⁻ˣ − sin x − 1 = 0` using the **Bisection method**. `[6]`

**(c)** Find a real root of the equation `x = e⁻ˣ` using the **Newton-Raphson method**, correct to 5 decimal places. `[5]`

---

## Question 2

**(a)** Solve by **Gauss-Elimination method**: `[7]`

```
3x +  y −  z =  3
2x − 8y +  z = −5
 x − 2y + 9z =  8
```

**(b)** Solve the following system by **Gauss-Jacobi method**: `[7]`

```
10x −  5y −  2z =  3
 4x − 10y +  3z = −3
  x +  6y + 10z = −3
```

---

## Question 3

**(a)** Evaluate `I = ∫₀¹ 1/(1 + x) dx` correct to three decimal places using both **Trapezoidal** and **Simpson's 1/3 rules** with h = 0.5, 0.25, 0.125. `[7]`

**(b)** From the following table of values of x and y, obtain `dy/dx` and `d²y/dx²` for x = 1.2: `[7]`

|x|1.0|1.2|1.4|1.6|1.8|2.0|2.2|
|---|---|---|---|---|---|---|---|
|y|2.7183|3.3201|4.0552|4.9530|6.0496|7.3891|9.0250|

---

## Question 4

**(a)** Discuss **Euler's method** to solve IVP. `[4]`

**(b)** Solve the equation `dy/dx = x + y`, y(0) = 1 at point x = 0.10. `[5]`

**(c)** Solve the boundary value problem `y'' − 64y + 10 = 0` with y(0) = y(1) = 0 by **finite difference method**. `[5]`

---

## Question 5

**(a)** Solve **Laplace's equation** `u_xx + u_yy = 0` for the following figure: `[7]`

```
        50    100   100   100    50
         |-----|-----|-----|-----|
    0 —  |  u₇ |  u₈ |  u₉ |  — 0
         |-----|-----|-----|-----|
    0 —  |  u₄ |  u₅ |  u₆ |  — 0
         |-----|-----|-----|-----|
    0 —  |  u₁ |  u₂ |  u₃ |  — 0
         |-----|-----|-----|-----|
              0     0     0     0
```

**(b)** Solve the **Poisson equation** `u_xx + u_yy = −10(x² + y² + 10)` in the domain of the following figure: `[7]`

```
Y
3 |———————u=0———————|
  |        |        |
2 |  C(u₃) | D(u₄)  |
  |--------|--------|
  |u=0     |        |u=0
  |  A(u₁) | B(u₂)  |
1 |--------|--------|
  |        |        |
0 |________|________|
  0    1    2    3   → X
```

Boundary condition: u = 0 on all four sides. Interior nodes: **A = (1,1)**, **B = (2,1)**, **C = (1,2)**, **D = (2,2)** Unknowns: u₁, u₂, u₃, u₄

---

## Question 6

**(a)** From the **Taylor series** for y(x), find y(0.1) correct to four decimal places if y(x) satisfies: `[6]`

$$y' = x - y^2, \quad y(0) = 1$$

**(b)** Using the **Modified Euler method**, find the value of y satisfying the equation `dy/dx = logₑ(x + y)` for x = 1.2 and x = 1.4, correct to four decimal places, take h = 0.2 and y(1) = 2. `[8]`

---

## Question 7

**(a)** Compute y(0.2) by **Runge-Kutta method of 4th order** for the differential equation: `[5]`

$$\frac{dy}{dx} = xy + y^2, \quad y(0) = 1$$

**(b)** The velocity v ms⁻¹ of a moving car is given at fixed intervals of time t (second) as follows. Find the distance covered by the car in 12 seconds. `[4]`

|t (s)|0|2|4|6|8|10|12|
|---|---|---|---|---|---|---|---|
|v (ms⁻¹)|4|6|16|34|60|94|136|

**(c)** What do you mean by interpolation? Apply **Lagrange's formula** to find the form of the function f(x) using the following table: `[4]`

|x|0|1|2|3|4|
|---|---|---|---|---|---|
|f(x)|3|6|11|18|27|

---

_End of Question Sets_