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

where ( y^*(x) ) is the approximate solution, ( c_i ) are unknown constants to be determined, and ( N_i(x) ) are **trial functions**.

The **trial functions** must be *admissible* — that is, continuous over the domain and satisfying the boundary conditions exactly.
They should also reflect the physics of the problem in a general sense.
Given these conditions, it is unlikely that Eq. (5.3) provides an exact solution.

Substituting Eq. (5.3) into the differential equation (5.1) gives a **residual function**:

$$
R(x) = D[y^*(x), x] \ne 0
\tag{5.4}
$$

where ( R(x) ) depends on the unknown coefficients ( c_i ).

The **method of weighted residuals** requires that the coefficients ( c_i ) be chosen such that the *weighted integral of the residual* over the domain vanishes:

$$
\int_{a}^{b} w_i(x) R(x), dx = 0, \quad i = 1, 2, \dots, n
\tag{5.5}
$$

Here ( w_i(x) ) are **arbitrary weighting functions**.
Equation (5.5) produces ( n ) algebraic equations, which can be solved for the ( n ) unknown coefficients ( c_i ).

This expresses that the *integral of the weighted residual error* over the domain is zero.
Because the trial functions satisfy the boundary conditions, the solution is exact at the endpoints, while the residual may be nonzero at interior points.

Several variations of the MWR exist, differing mainly in how the weighting functions ( w_i(x) ) are chosen.
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
\int_{a}^{b} w_i(x), R(x), dx =
\int_{a}^{b} N_i(x), R(x), dx = 0, \quad i = 1, 2, \dots, n
\tag{5.7}
$$

This again results in ( n ) algebraic equations for evaluation of the unknown coefficients ( c_i ).
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

## Galerkin Finite Element Method

In **Galerkin’s method**, the **weight functions are chosen identical to the trial functions**:

$$
w_j(x) = \phi_j(x) \tag{3.4}
$$

The residual equation becomes:

$$
\int_\Omega \phi_j(x) \left[ L\left(\sum_{i=1}^n a_i \phi_i(x)\right) - f(x) \right] dx = 0, \quad j=1,2,\dots,n \tag{3.5}
$$

**Procedure:**

1. Choose appropriate **trial functions** \(\phi_i(x)\) satisfying **essential boundary conditions**.  
2. Compute **residual** \(R(x)\).  
3. Multiply residual by \(\phi_j(x)\) and integrate over the domain.  
4. Solve resulting **linear system** for \(a_i\).  

---

## Application to Structural Elements

Consider a **bar element** subjected to axial load:

* Governing differential equation:

$$
\frac{d}{dx} \left( EA \frac{du}{dx} \right) + q(x) = 0, \quad 0 < x < L \tag{3.6}
$$

* Boundary conditions:

$$
u(0) = u_0, \quad EA \frac{du}{dx}\Big|_{x=L} = P \tag{3.7}
$$

### Galerkin Formulation

* Trial function:

$$
u^*(x) = \sum_{i=1}^{n} a_i \phi_i(x) \tag{3.8}
$$

* Weighted residual equation:

$$
\int_0^L \phi_j(x) \left[ \frac{d}{dx} \left( EA \frac{du^*}{dx} \right) + q(x) \right] dx = 0, \quad j=1,2,\dots,n \tag{3.9}
$$

* Integrate by parts to **reduce order of derivative**:

$$
\int_0^L EA \frac{du^*}{dx} \frac{d\phi_j}{dx} dx - \left[ \phi_j EA \frac{du^*}{dx} \right]_0^L + \int_0^L \phi_j q(x) dx = 0 \tag{3.10}
$$

* Boundary term vanishes for **essential BCs**, leaving **stiffness matrix form**:

$$
[K]\{a\} = \{F\} \tag{3.11}
$$

---

## Interpolation Functions

* Interpolation (shape) functions \(\phi_i(x)\) approximate the **field variable** within an element.  
* They must satisfy:

1. **Nodal property**: \(\phi_i(x_j) = \delta_{ij}\)  
2. **Completeness**: ability to represent constant, linear, etc., variations  
3. **Continuity**: compatible across element boundaries  

**Linear 1D element (bar):**

$$
\phi_1(x) = 1 - \frac{x}{L}, \quad \phi_2(x) = \frac{x}{L} \tag{3.12}
$$

**Cubic 1D element (beam/flexure):**

$$
v(x) = N_1(x)v_1 + N_2(x)\theta_1 + N_3(x)v_2 + N_4(x)\theta_2 \tag{3.13}
$$

---

## Compatibility and Completeness Requirements

**1. Compatibility:**  

* Displacements must be **continuous at element interfaces**.  
* No sudden jumps in \(u(x)\) or \(v(x)\).  

**2. Completeness:**  

* Element must represent **rigid body motion** and **constant strain states**.  
* For 1D linear bar: can represent \(u(x) = a + bx\) exactly.  
* For 1D beam: cubic polynomials required to satisfy bending behavior.  

*Failure to satisfy either condition may lead to inaccurate results or **element locking**.*

---

## Polynomial Form Applications

* **Polynomial trial functions** are widely used due to simplicity.  
* Degree of polynomial determines **accuracy**:

| Polynomial Degree | Element Type | Number of Nodes |
| ----------------- | ----------- | --------------- |
| 1 (linear)        | bar         | 2               |
| 2 (quadratic)     | bar         | 3               |
| 3 (cubic)         | beam        | 2               |

*Interpolation within element:*

$$
u^*(x) = a_0 + a_1 x + a_2 x^2 + \dots + a_m x^m \tag{3.14}
$$

*Coefficients \(a_i\) determined using **Galerkin weighted residual**.*

**Example: 2-node beam element (cubic):**

$$
v(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3 \tag{3.15}
$$

*Apply nodal displacements and slopes to solve for \(a_i\), similar to Unit 2 flexure element.*

---

## 📚 Reference


- [*Fundamentals of Finite Element Analysis - (Unit 3 - Chapter 5)*](Resources/FEM_Hutton_Unit3_Ch5.pdf) – Hutton David, McGraw-Hill 
- [*Fundamentals of Finite Element Analysis - (Unit 3 and 4 - Chapter 6)*](Resources/FEM_Hutton_Unit3&4_Ch6.pdf) – Hutton David, McGraw-Hill 
