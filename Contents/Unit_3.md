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
\int_{a}^{b} w_i(x) R(x) dx =
\int_{a}^{b} N_i(x) R(x) dx = 0, \quad i = 1, 2, \dots, n
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

### Example 5.3

**Problem:**  
Use **Galerkin’s method of weighted residuals** to obtain a one-term approximation to the solution of the differential equation

$$
\frac{d^2 y}{dx^2} + y = 4x, \qquad 0 \le x \le 1
$$

with boundary conditions
$$
y(0) = 0, \qquad y(1) = 1.
$$

**Solution:**  

Here the boundary conditions are **nonhomogeneous**, so the trial solution must be constructed to satisfy the prescribed boundary values.  
We assume a trial solution of the form

$$
y^*(x) = c_1 N_1(x) + f(x)
$$

where \(N_1(x)\) satisfies the homogeneous boundary conditions and \(f(x)\) is chosen to satisfy the nonhomogeneous condition.  
Choose

$$
N_1(x) = x(x-1), \qquad f(x) = x,
$$

so that \(y^*(0)=0\) and \(y^*(1)=1\) are satisfied identically. Thus

$$
y^*(x) = c_1 x(x-1) + x.
$$

Compute derivatives:

$$
\frac{d^2 y^*}{dx^2} = 2 c_1.
$$

Form the residual

$$
R(x;c_1) = \frac{d^2 y^*}{dx^2} + y^* - 4x
= 2c_1 + c_1(x^2 - x) + x - 4x
= c_1 x^2 - c_1 x + 2c_1 - 3x.
$$

Apply Galerkin’s weighted residual condition with weight \(N_1(x)\):

$$
\int_{0}^{1} N_1(x)\, R(x;c_1)\, dx = 0,
$$

i.e.

$$
\int_{0}^{1} x(x-1)\big( c_1 x^2 - c_1 x + 2c_1 - 3x \big)\, dx = 0.
$$

Carry out the integration (algebra steps):

1. Expand integrand:
   $$
   x(x-1)\big( c_1 x^2 - c_1 x + 2c_1 - 3x \big)
   = c_1(x^4 - x^3 - x^3 + x^2) + 2c_1(x^2 - x) - 3(x^2 - x).
   $$

2. Integrate term-by-term from 0 to 1. The symbolic evaluation gives the linear equation in \(c_1\):
   $$
   \frac{1}{4} - \frac{3}{10} c_1 = 0.
   $$

Solve for \(c_1\):

$$
c_1 = \frac{1/4}{3/10} = \frac{10}{12} = \frac{5}{6}.
$$

Hence the one-term Galerkin approximate solution is

$$
y^*(x) = \frac{5}{6}\, x(x-1) + x
= \frac{5}{6}x^2 + \frac{1}{6}x.
$$

**Exact solution (for comparison):**  
Solving the differential equation exactly yields

$$
y(x) = C_1\sin x + C_2\cos x + 4x.
$$

Applying the boundary conditions \(y(0)=0\) and \(y(1)=1\) gives \(C_2=0\) and

$$
C_1 = \frac{1-4}{\sin 1} = -\frac{3}{\sin 1} \approx -3.565,
$$

so the exact solution may be written

$$
y(x) = 4x - \frac{3}{\sin 1}\,\sin x.
$$

<img width="700" height="576" alt="image" src="https://github.com/user-attachments/assets/f5bb4e20-e45f-48a4-bccc-4c178963cdf0" />

**Observation:**  

A plot of the approximate and exact solutions (see Figure 5.3 in Hutton) shows reasonable agreement for the one-term Galerkin approximation. The approximate solution satisfies the boundary conditions exactly (by construction). The match can be improved by including additional trial functions (higher-order terms), which allows the Galerkin approximation to capture the influence of the sinusoidal components in the exact solution more closely.

---

> **Note on Accuracy and Convergence in the Method of Weighted Residuals**
>
> A natural question arises after obtaining an approximate solution:  
> **How do we know when the Method of Weighted Residuals (MWR) solution is accurate enough?**  
> In other words, how can we determine whether our approximate solution is sufficiently close to the exact one?

> This issue of **convergence** is central to all approximate solution techniques.  
> In most practical problems, the **exact solution is unknown**, so we must develop some logical criterion to assess accuracy.
>
> The general approach in MWR is **progressive refinement**:
> - Begin with a small number of trial functions and obtain the corresponding approximate solution.  
> - Gradually **increase the number of trial functions** and observe how the solution changes.  
> - If the solution varies only slightly with the addition of more trial functions, we say that the **solution has converged**.
>
> However, convergence alone does not guarantee correctness—i.e., that the approximation converges **to the true solution**.  
> This deeper question involves advanced theoretical analysis, which lies beyond the present discussion.  
> For practical engineering purposes, we assume that if the solution converges smoothly, it converges toward the correct physical behavior.
>
> To ensure that a converged numerical solution is *reasonable* in the context of a physical problem, one can perform additional checks such as:
> - **Equilibrium verification** (for structural or mechanical systems),  
> - **Energy balance** checks,  
> - **Heat or fluid flow balance**, and other problem-specific validations discussed in later chapters.
>
> In the earlier examples, we employed **trial functions chosen ad hoc**—that is, functions tailored to satisfy the boundary conditions but not derived from a systematic procedure.  
> While this approach is perfectly valid, a more structured method involves **polynomial trial functions**, which provide a systematic means of increasing the number of terms and examining convergence behavior more rigorously.
>
> The following example illustrates this polynomial-based approach to the Method of Weighted Residuals.

---

### **Example 5.4**

**Problem:**  
Solve the problem of Examples 5.1 and 5.2 by assuming a general polynomial form for the solution as  

$$
y^*(x) = c_0 + c_1x + c_2x^2 + \cdots
$$

**Solution:**  
For the first trial, consider only the quadratic form:

$$
y^*(x) = c_0 + c_1x + c_2x^2
$$

Applying the boundary conditions \( y(0) = 0 \) and \( y(1) = 0 \):

$$
\begin{aligned}
y^*(0) &= 0 = c_0, \\
y^*(1) &= 0 = c_1 + c_2.
\end{aligned}
$$

The second equation shows that \( c_1 \) and \( c_2 \) are **not independent** if the homogeneous boundary conditions are to be satisfied exactly.  
This gives the constraint relation:

$$
c_2 = -c_1
$$

Thus, the trial solution becomes

$$
y^*(x) = c_1x + c_2x^2 = c_1x - c_1x^2 = c_1x(1 - x)
$$

which is identical to the one-term solution obtained in **Example 5.1**.

Next, we **add a cubic term** and write:

$$
y^*(x) = c_0 + c_1x + c_2x^2 + c_3x^3
$$

Applying the boundary conditions:

$$
\begin{aligned}
y^*(0) &= 0 = c_0, \\
y^*(1) &= 0 = c_1 + c_2 + c_3.
\end{aligned}
$$

Hence, we have the constraint relation:

$$
c_1 + c_2 + c_3 = 0
$$

or equivalently,

$$
c_3 = - (c_1 + c_2)
$$

Substituting this back, the trial solution becomes

$$
\begin{aligned}
y^*(x) &= c_1x + c_2x^2 + c_3x^3 \\
       &= c_1x + c_2x^2 - (c_1 + c_2)x^3 \\
       &= c_1x(1 - x^2) + c_2x^2(1 - x)
\end{aligned}
$$

Thus, we have obtained **two trial functions**, each satisfying the boundary conditions identically.  
(The determination of constants for this two-term solution is left as an end-of-chapter exercise.)

Now, **add the quartic term** and consider the trial solution:

$$
y^*(x) = c_0 + c_1x + c_2x^2 + c_3x^3 + c_4x^4
$$

Applying the boundary conditions:

$$
\begin{aligned}
c_0 &= 0, \\
c_1 + c_2 + c_3 + c_4 &= 0
\end{aligned}
$$

We can use this constraint to eliminate \( c_4 \) (arbitrarily chosen) and rewrite:

$$
\begin{aligned}
y^*(x) &= c_1x + c_2x^2 + c_3x^3 - (c_1 + c_2 + c_3)x^4 \\
       &= c_1x(1 - x^3) + c_2x^2(1 - x^2) + c_3x^3(1 - x)
\end{aligned}
$$

**Residual Formulation**

Substituting into the governing differential equation

$$
\frac{d^2y}{dx^2} = 10x^2 + 5
$$

we obtain the residual:

$$
R(x; c_1, c_2, c_3) = -12c_1x^2 + c_2(2 - 12x^2) + c_3(6x - 12x^2) - 10x^2 - 5
$$

Setting the residual expression equal to zero and equating coefficients of like powers of \( x \),  
we find that the residual is exactly zero when:

$$
\begin{aligned}
c_1 &= -\frac{10}{3}, \\
c_2 &= \frac{5}{2}, \\
c_3 &= 0, \\
c_4 &= \frac{5}{6}
\end{aligned}
$$

**Final Solution**

Hence, the resulting polynomial form gives the **exact solution**:

$$
y^*(x) = \frac{5}{6}x^4 + \frac{5}{2}x^2 - \frac{10}{3}x
$$

This demonstrates that by **increasing the polynomial order**,  
the **Galerkin formulation using the Method of Weighted Residuals** can recover the **exact analytical solution** for this problem.

---

> **Note:**  
> The procedure detailed in the previous example represents a **systematic approach** for developing polynomial trial functions.  
> It can be applied not only to problems with **homogeneous boundary conditions**, but also extended to those with **nonhomogeneous conditions** by appropriate modification of the trial function form.  
> 
> Algebraically, the process is quite **straightforward**, but it becomes increasingly **tedious** as the number of trial functions — and thus the **order of the polynomial** — is increased.  
> 
> Having now outlined the **general technique of Galerkin’s Method of Weighted Residuals (MWR)**, we proceed next to develop the **Galerkin Finite Element Method**, which is founded on the same fundamental principles of MWR.

---

## Galerkin Finite Element Method

The **classical method of weighted residuals** described earlier utilizes **global trial functions**—that is, each trial function must apply over the **entire domain** and satisfy all **boundary conditions** exactly.  

However, in practical **two- and three-dimensional problems**, governed by **partial differential equations**, determining suitable trial functions and verifying their accuracy become formidable tasks.  

To overcome this, the **Galerkin approach** is adapted to the **finite element context**, where local trial functions are defined over smaller subdomains (elements).

### **Governing Equation**

Consider the differential equation:

$$
\frac{d^2 y}{dx^2} + f(x) = 0
\quad \text{subject to} \quad
y(a) = y_a, \quad y(b) = y_b
\tag{5.8, 5.9}
$$

The problem domain is divided into **M elements**, bounded by **M + 1 nodal points**  (Figure 5.4a) $$x_1, x_2, \dots, x_{M+1}$$, where:

$$
x_1 = x_a, \quad x_{M+1} = x_b
$$

<img width="1015" height="763" alt="image" src="https://github.com/user-attachments/assets/9d7b28b2-31d6-48bf-8a28-c67b9bf30923" />


### **Approximate Solution**

An approximate solution is assumed in the form:

$$
y^*(x) = \sum_{i=1}^{M+1} y_i \, n_i(x)
\tag{5.10}
$$

where:
- $$y_i$$ = value of the solution at node $$\( x_i \)$$,
- $$n_i(x)$$ = corresponding **trial (shape) function**.

In contrast to the global trial functions of MWR, here the $$n_i(x)$$ are **piecewise-defined and locally supported**, i.e., **nonzero only within one or two adjacent elements**.

### **Shape Functions**

For a simple **linear element**, the trial (interpolation) functions are defined as:

$$
n_i(x) =
\begin{cases}
\dfrac{x - x_{i-1}}{x_i - x_{i-1}}, & x_{i-1} \le x \le x_i \\
[6pt]
\dfrac{x_{i+1} - x}{x_{i+1} - x_i}, & x_i \le x \le x_{i+1} \\
[6pt]
0, & \text{otherwise}
\end{cases}
\tag{5.11}
$$

Thus, each $$n_i(x)$$ overlaps only with its immediate neighbors $$n_{i-1}(x)$$ and $$n_{i+1}(x)$$, as illustrated schematically in **Figure 5.4(b)**.

### **Local Approximation Example**

In the interval $$x_2 \le x \le x_3$$, only two functions— $$n_2(x)$$ and $$n_3(x$$ —are nonzero.  
The approximate solution becomes:

$$
y^*(x) = y_2 n_2(x) + y_3 n_3(x)
= y_2 \frac{x_3 - x}{x_3 - x_2} + y_3 \frac{x - x_2}{x_3 - x_2}
\tag{5.12}
$$

> Here, $$y_2$$ and $$y_3$$ are the nodal values at points $$x_2$$ and $$x_3$$, and the solution varies **linearly** within the element.

### **Weighted Residual Formulation**

Substituting the approximate solution (Eq. 5.10) into the governing equation (Eq. 5.8), we obtain the **residual**:

$$
R(x; y_i) =
\frac{d^2 y^*}{dx^2} + f(x)
= \sum_{i=1}^{M+1} \frac{d^2}{dx^2} \{ y_i n_i(x) \} + f(x)
\tag{5.13}
$$

Applying **Galerkin’s weighted residual principle**, each trial function is used as a **weighting function**, giving:

$$
\int_{x_a}^{x_b} n_j(x) R(x; y_i) \, dx = 0
\quad j = 1, 2, \dots, M+1
\tag{5.14}
$$

That is,

$$
\int_{x_a}^{x_b} n_j(x)
\left[
\sum_{i=1}^{M+1} \frac{d^2}{dx^2} \{ y_i n_i(x) \} + f(x)
\right] dx = 0
$$

### **Element-Level Expression**

In any single element $$x_j \le x \le x_{j+1}$$, only **two shape functions**— $$n_j(x) \) and \( n_{j+1}(x)$$ —are nonzero.  
Hence, the equation reduces to:

$$
\int_{x_j}^{x_{j+1}}
n_j(x)
\left[
\frac{d^2}{dx^2} (y_j n_j(x) + y_{j+1} n_{j+1}(x)) + f(x)
\right] dx = 0,
\quad j = 1, 2, \dots, M
\tag{5.15}
$$

### **Matrix Form**

After integration over all elements, we obtain a system of **M + 1 algebraic equations**:

$$
[K]\{y\} = \{F\}
\tag{5.16}
$$

where:  
- $$[K]$$ = global **stiffness matrix**,  
- $${y}$$ = vector of nodal **displacements (solution values)**,  
- $${F}$$ = vector of nodal **forces (load terms)**.

### **Observation**

Equation (5.14) represents the **formal statement of the Galerkin Finite Element Method**.  
It clearly demonstrates that the **finite element formulation** is rooted in the **Method of Weighted Residuals**, but differs by:
- Using **piecewise-defined trial functions**, and  
- Performing integration **locally over each element**.

This approach leads naturally to the **element formation** and **system assembly** procedures that form the foundation of all **finite element formulations**.

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
