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

---

## Introduction

The **Method of Weighted Residuals (MWR)** is a general procedure for obtaining approximate solutions to **differential equations** that govern structural behavior.  

*Exact solutions* are often difficult or impossible to obtain for complex geometries, loading, or boundary conditions. **MWR converts a differential equation problem into a system of algebraic equations**, suitable for numerical computation.  

---

## Weighted Residual Methods

Consider a **boundary value problem**:

$$
L[u(x)] = f(x), \quad x \in \Omega
$$

with boundary conditions:

$$
B[u(x)] = g(x), \quad x \in \partial \Omega
$$

where:  

* \(L\) = differential operator  
* \(u(x)\) = unknown function  
* \(f(x)\) = known function (forcing)  
* \(\Omega\) = domain of interest  
* \(B\) = boundary operator  

### Approximate Solution

Assume an approximate solution:

$$
u^*(x) = \sum_{i=1}^{n} a_i \phi_i(x) \tag{3.1}
$$

where:

* \(a_i\) = unknown coefficients  
* \(\phi_i(x)\) = chosen trial (basis) functions  

**Residual** is defined as:

$$
R(x) = L[u^*(x)] - f(x) \tag{3.2}
$$

The **weighted residual method** requires that:

$$
\int_\Omega w_j(x) R(x) \, dx = 0, \quad j = 1,2,\dots,n \tag{3.3}
$$

where \(w_j(x)\) are **weight functions**. Different choices of \(w_j\) produce different methods:

* Collocation: \(w_j(x) = \delta(x - x_j)\)  
* Subdomain: integral over subdomain  
* Least-squares: \(w_j(x) = \frac{dR}{da_j}\)  
* Galerkin: \(w_j(x) = \phi_j(x)\)  

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
