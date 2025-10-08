<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>

<script type="text/javascript"
        src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

# MST-102: Finite Element Method in Structural Engineering  

## Unit 3: Method of Weighted Residuals

- [Introduction](#introduction)  
- [Weighted Residual Methods](#weighted-residual-methods)  
- [Galerkin Finite Element Method](#galerkin-finite-element-method)  
- [Application to Structural Elements](#application-to-structural-elements)  
- [Interpolation Functions](#interpolation-functions)  
- [Compatibility and Completeness Requirements](#compatibility-and-completeness-requirements)  
- [Polynomial Form Applications](#polynomial-form-applications)  

## **Introduction**

In the previous units, the **fundamental concepts** of the Finite Element Method (FEM) were developed using **line elements** such as the *spring*, *bar*, and *beam (flexure)* elements. These elements are termed **line elements** because their properties and behavior can be represented using a **single spatial coordinate** along their longitudinal axis.  

For such elements, the **displacement–force relationships** can be readily derived using basic principles of **strength of materials**. However, when extending FEM to more complex problems—such as **two-dimensional continua**, **three-dimensional solids**, or **non-structural field problems** (e.g., heat transfer, fluid flow, diffusion)—the governing **differential equations** become more intricate.  

To address these general cases, FEM relies on a **mathematical framework** that allows approximate solutions to differential equations while satisfying boundary conditions. One such framework is the **Method of Weighted Residuals (MWR)**.  

The **MWR** provides a unified approach to derive finite element equations for any field problem governed by differential equations. Among its various forms, **Galerkin’s Method** is particularly significant, as it forms the foundation of most **finite element formulations** used in structural engineering and beyond.  

**Key Concept:**  
_The **Method of Weighted Residuals (MWR)** transforms a governing differential equation into a **set of algebraic equations** by enforcing that the **residual (error)** is orthogonal to a chosen set of **weighting functions**, leading to an approximate but highly practical solution form._

---

## Weighted Residual Methods

It is a basic fact that most practical problems in engineering are governed by **differential equations**.
Owing to complexities of geometry and loading, rarely are exact solutions to the governing equations possible.
Therefore, **approximate techniques** for solving differential equations are indispensable in engineering analysis.

Indeed, the **finite element method (FEM)** is such a technique.
However, FEM itself is based on several more fundamental approximate methods — one of which is discussed in this section and subsequently applied to finite element formulation.

The **Method of Weighted Residuals (MWR)** is an approximate technique for solving **boundary value problems** that uses *trial functions* satisfying the prescribed boundary conditions and an *integral formulation* to minimize error, in an average sense, over the problem domain.

The concept is described here for the one-dimensional case, though extension to two and three dimensions is straightforward.

Given a differential equation of the general form

$$
D[y(x), x] = 0, \quad a < x < b
\tag{5.1}
$$

subject to **homogeneous boundary conditions**

$$
y(a) = y(b) = 0
\tag{5.2}
$$

the MWR seeks an approximate solution in the form

$$
y^*(x) = \sum_{i=1}^{n} c_i N_i(x)
\tag{5.3}
$$

where $y^*(x)$ is the approximate solution, $c_i$ are unknown constants to be determined, and $N_i(x)$ are **trial functions**.

The **trial functions** must be *admissible* — that is, continuous over the domain and satisfying the boundary conditions exactly.
They should also reflect the physics of the problem in a general sense.
Given these conditions, it is unlikely that Eq. (5.3) provides an exact solution.

Substituting Eq. (5.3) into the differential equation (5.1) gives a **residual function**:

$$
R(x) = D[y^*(x), x] \ne 0
\tag{5.4}
$$

where $R(x)$ depends on the unknown coefficients $c_i$.

The **method of weighted residuals** requires that the coefficients $c_i$ be chosen such that the *weighted integral of the residual* over the domain vanishes:

$$
\int_{a}^{b} w_i(x) R(x), dx = 0, \quad i = 1, 2, \dots, n
\tag{5.5}
$$

Here $w_i(x)$ are **arbitrary weighting functions**.
Equation (5.5) produces $n$ algebraic equations, which can be solved for the $n$ unknown coefficients $c_i$.

This expresses that the *integral of the weighted residual error* over the domain is zero.
Because the trial functions satisfy the boundary conditions, the solution is exact at the endpoints, while the residual may be nonzero at interior points.

Several variations of the MWR exist, differing mainly in how the weighting functions $w_i(x)$ are chosen.
The most common are:

* Point Collocation
* Subdomain Collocation
* Least Squares
* **Galerkin Method**

Since the **Galerkin method** is simple and well-suited to finite element formulation, it is discussed in detail next.

In **Galerkin’s Weighted Residual Method**, the weighting functions are taken identical to the trial functions:

$$
w_i(x) = N_i(x), \quad i = 1, 2, \dots, n
\tag{5.6}
$$

Therefore, the unknown parameters are determined via

$$
\int_{a}^{b} w_i(x) R(x), dx =
\int_{a}^{b} N_i(x) R(x), dx = 0, \quad i = 1, 2, \dots, n
\tag{5.7}
$$

This again results in $n$ algebraic equations for evaluation of the unknown coefficients $c_i$.
The following examples illustrate the procedure in detail.

---

### Example 5.1

**Problem:**  
Use **Galerkin’s method of weighted residuals** to obtain an approximate solution of the differential equation  

$$
\frac{d^2 y}{dx^2} - 10x^2 = 5, \quad 0 \le x \le 1
$$  

with boundary conditions  

$$
y(0) = y(1) = 0.
$$  

**Solution:**  

The presence of the quadratic term in the differential equation suggests that **trial functions in polynomial form** are suitable. For homogeneous boundary conditions at \(x = 0\) and \(x = 1\), the general form  

$$
N(x) = (x - x_a)^p (x - x_b)^q
$$  

with \(p\) and \(q\) being positive integers greater than zero, automatically satisfies the boundary conditions and is continuous in \(x_a \le x \le x_b\).  

Using a **single trial function**, the simplest form that satisfies the boundary conditions is  

$$
N_1(x) = x(x-1)
$$  

The approximate solution per Equation (5.3) is  

$$
y^*(x) = c_1 x(x-1)
$$  

with derivatives  

$$
\frac{dy^*}{dx} = c_1 (2x - 1), \quad \frac{d^2 y^*}{dx^2} = 2c_1
$$  

> Note: At this point, the selected trial function does not satisfy the physics exactly because the second derivative is constant, whereas the differential equation requires a quadratic function of \(x\). Nonetheless, we continue to illustrate the method.  

Substituting \(\frac{d^2 y^*}{dx^2}\) into the differential equation gives the **residual**  

$$
R(x;c_1) = 2c_1 - 10x^2 - 5
$$  

Applying Galerkin’s method, we solve  

$$
\int_0^1 x(x-1) \, R(x;c_1) \, dx = 0
$$  

which after integration yields  

$$
c_1 = 4
$$  

Hence, the **approximate solution** is  

$$
y^*(x) = 4 x(x-1)
$$  

**Exact solution:**  
Integrating the differential equation twice gives  

$$
\frac{dy}{dx} = \int \left( \frac{d^2 y}{dx^2} \right) dx = \int (10x^2 + 5) dx = \frac{10x^3}{3} + 5x + C_1
$$  

$$
y(x) = \int \frac{dy}{dx} dx = \frac{5x^4}{6} + \frac{5x^2}{2} + C_1 x + C_2
$$  

Applying the boundary conditions:  

$$
y(0) = 0 \implies C_2 = 0
$$  

$$
y(1) = 0 \implies \frac{5}{6} + \frac{5}{2} + C_1 = 0 \implies C_1 = -\frac{10}{3}
$$  

Thus, the **exact solution** is  

$$
y(x) = \frac{5x^4}{6} + \frac{5x^2}{2} - \frac{10}{3} x
$$  

<img width="745" height="592" alt="image" src="https://github.com/user-attachments/assets/f6e68039-92d0-449d-8540-e3c6653a796b" />


**Observation:**  

A graphical comparison of the approximate and exact solutions (see Figure 5.1) shows that the approximate solution is in reasonable agreement with the exact solution. However, the **one-term approximate solution is symmetric**, while the exact solution is **asymmetric**, due to the quadratic term \(10x^2\) driving the solution.  

Despite this, the method gives a **good first approximation**, and including additional trial functions or higher-order polynomials would allow the solution to **closely approach the exact behavior** of the system.

---

### Example 5.2

**Problem:**  
Obtain a **two-term Galerkin solution** for the problem of Example 5.1 using the trial functions  

$$
N_1(x) = x(x-1), \quad N_2(x) = x^2(x-1)
$$  

**Solution:**  

The **two-term approximate solution** is  

$$
y^*(x) = c_1 x(x-1) + c_2 x^2(x-1)
$$  

The second derivative is  

$$
\frac{d^2 y^*}{dx^2} = 2c_1 + 2c_2 (3x-1)
$$  

Substituting into the differential equation, the **residual** is  

$$
R(x; c_1, c_2) = 2c_1 + 2c_2 (3x-1) - 10x^2 - 5
$$  

Using Galerkin’s method (trial functions as weighting functions), the residual equations are  

$$
\int_0^1 x(x-1) \, R(x; c_1, c_2) \, dx = 0
$$  

$$
\int_0^1 x^2(x-1) \, R(x; c_1, c_2) \, dx = 0
$$  

After integration and simplification, the algebraic equations become  

$$
6 c_1 - 2 c_2 = 19
$$  

$$
4 c_1 + 6 c_2 = 5
$$  

Solving simultaneously:  

$$
c_1 = \frac{19}{6}, \quad c_2 = \frac{5}{3}
$$  

Hence, the **two-term approximate solution** is  

$$
y^*(x) = \frac{19}{6} x(x-1) + \frac{5}{3} x^2(x-1) = \frac{5}{3} x^3 + \frac{3}{2} x^2 - \frac{19}{6} x
$$  

<img width="673" height="540" alt="image" src="https://github.com/user-attachments/assets/e7bfa38e-d663-403d-bbf8-f51941d34c04" />

**Observation:**  

For comparison, the **exact solution**, the **one-term**, and the **two-term solutions** can be plotted (Figure 5.2). The two-term Galerkin solution is **almost indistinguishable** from the exact solution, showing how adding higher-order terms improves the accuracy of the approximate solution.

---
## Galerkin Finite Element Method

---

## Application to Structural Elements

---

## Interpolation Functions

---

## Compatibility and Completeness Requirements

---

## Polynomial Form Applications


---

## 📚 References


- [*Fundamentals of Finite Element Analysis - (Unit 3 - Chapter 5)*](Resources/FEM_Hutton_Unit3_Ch5.pdf) – Hutton David, McGraw-Hill 
- [*Fundamentals of Finite Element Analysis - (Unit 3 and 4 - Chapter 6)*](Resources/FEM_Hutton_Unit3&4_Ch6.pdf) – Hutton David, McGraw-Hill 
