# Numerical Integration: Trapezoidal & Simpson's Rules

This note covers the formulas, core concepts, and algorithmic breakdowns for three fundamental Newton-Cotes closed quadrature formulas used for numerical integration.

---

## 1. Trapezoidal Method
The Trapezoidal Rule approximates the area under a curve by dividing the total area into a series of **trapezoids** rather than rectangles. It assumes a linear (straight-line) relationship between adjacent data points.



### 📐 Formula
For an interval $[a, b]$ divided into $n$ equal sub-intervals of width $h = \frac{b - a}{n}$:

$$\int_{a}^{b} f(x) \, dx \approx \frac{h}{2} \left[ (y_0 + y_n) + 2(y_1 + y_2 + y_3 + \dots + y_{n-1}) \right]$$

* **In words:** $\frac{h}{2} \times [(\text{First } y + \text{Last } y) + 2 \times (\text{Sum of remaining } y\text{'s})]$
* **Limitation/Constraint:** $n$ (number of intervals) can be **any integer** (even or odd).

### 🛠️ Execution Blueprint
1. **Calculate Interval Width ($h$):** $h = \frac{b - a}{n}$
2. **Generate Table:** Compute $x_i = a + ih$ and their corresponding $y_i = f(x_i)$ values from $i = 0$ to $n$.
3. **Plug & Chug:** Separate $y_0$ and $y_n$ from the interior coordinates, double the interior sum, and multiply the entire aggregate by $\frac{h}{2}$.

---

## 2. Simpson's 1/3 Rule
Simpson's 1/3 Rule provides a more accurate approximation by connecting three consecutive points with a **parabolic arc** (2nd-degree polynomial) instead of straight lines.



### 📐 Formula
For an interval $[a, b]$ divided into $n$ equal sub-intervals of width $h$:

$$\int_{a}^{b} f(x) \, dx \approx \frac{h}{3} \left[ (y_0 + y_n) + 4(y_1 + y_3 + \dots + y_{n-1}) + 2(y_2 + y_4 + \dots + y_{n-2}) \right]$$

* **In words:** $\frac{h}{3} \times [(\text{First } y + \text{Last } y) + 4 \times (\text{Sum of ODD } y\text{'s}) + 2 \times (\text{Sum of EVEN } y\text{'s})]$
* **⚠️ CRITICAL CONSTRAINT:** The total number of intervals ($n$) **MUST BE EVEN** ($n = 2, 4, 6, 8, \dots$). If $n$ is odd, this method fails.

### 🛠️ Execution Blueprint
1. Check if $n$ is even. Find $h = \frac{b - a}{n}$.
2. Create the $x$ and $y$ coordinate data array.
3. Group your boundaries ($y_0, y_n$), your odd-indexed steps ($y_1, y_3, \dots$), and your even-indexed steps ($y_2, y_4, \dots$).
4. Apply the weights ($1, 4, 2$ pattern) and multiply by $\frac{h}{3}$.

---

## 3. Simpson's 3/8 Rule
Simpson's 3/8 Rule fits a **cubic polynomial** (3rd-degree curve) through groups of four consecutive points. It is slightly more accurate than the 1/3 rule but requires tighter interval pairing.

### 📐 Formula
For an interval $[a, b]$ divided into $n$ equal sub-intervals of width $h$:

$$\int_{a}^{b} f(x) \, dx \approx \frac{3h}{8} \left[ (y_0 + y_n) + 3(y_1 + y_2 + y_4 + y_5 + \dots + y_{n-1}) + 2(y_3 + y_6 + \dots + y_{n-3}) \right]$$

* **In words:** $\frac{3h}{8} \times [(\text{First } y + \text{Last } y) + 3 \times (\text{Sum of positions NOT divisible by 3}) + 2 \times (\text{Sum of positions DIVISIBLE by 3})]$
* **⚠️ CRITICAL CONSTRAINT:** The total number of intervals ($n$) **MUST BE A MULTIPLE OF 3** ($n = 3, 6, 9, 12, \dots$).

### 🛠️ Execution Blueprint
1. Check if $n \pmod 3 == 0$. Find $h = \frac{b - a}{n}$.
2. Populate the $(x_i, y_i)$ baseline matrix.
3. Group the components:
   * Extremes: $y_0 + y_n$
   * Multiples of 3: $y_3 + y_6 + y_9 + \dots$ (Multiply this bucket by $2$)
   * Remaining indices: All other intermediate $y$ values (Multiply this bucket by $3$)
4. Compute the final value using the scaling multiplier $\frac{3h}{8}$.

---

## 📊 Summary Comparison for Exams

| Method | Order of Polynomial | Minimum / Step Constraint ($n$) | Multiplier Coefficient |
| :--- | :--- | :--- | :--- |
| **Trapezoidal** | 1st Order (Linear) | $n \ge 1$ (Any integer) | $\frac{h}{2}$ |
| **Simpson's 1/3** | 2nd Order (Quadratic) | $n$ must be **Even** (multiple of 2) | $\frac{h}{3}$ |
| **Simpson's 3/8** | 3rd Order (Cubic) | $n$ must be a **Multiple of 3** | $\frac{3h}{8}$ |