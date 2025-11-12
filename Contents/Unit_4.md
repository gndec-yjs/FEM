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

## Unit 4: Finite Element Types and Numerical Integration

**Types:** Triangular Elements, Rectangular Elements, Three-dimensional elements, Iso-parametric Formulation, Axis-Symmetric elements, Numerical integration, Gaussian quadrature

1. [Introduction](#41-introduction)
2. [Triangular Elements](#42-triangular-elements)
   - [Linear (CST) Triangular Element](#421-linear-cst-triangular-element)
   - [Quadratic (LST) Triangular Element](#422-quadratic-lst-triangular-element)
3. [Rectangular Elements](#43-rectangular-elements)
   - [Bilinear Quadrilateral Element (Q4)](#431-bilinear-quadrilateral-element-q4)
   - [Higher-Order Quadrilateral Elements](#432-higher-order-quadrilateral-elements)
4. [Three-Dimensional Elements](#44-three-dimensional-elements)
   - [Tetrahedral Element](#441-tetrahedral-element)
   - [Hexahedral (Brick) Element](#442-hexahedral-brick-element)
5. [Iso-parametric Formulation](#45-iso-parametric-formulation)
   - [Concept of Iso-parametric Elements](#451-concept-of-iso-parametric-elements)
   - [Jacobian Matrix and Coordinate Transformation](#452-jacobian-matrix-and-coordinate-transformation)
6. [Axis-Symmetric Elements](#46-axis-symmetric-elements)
7. [Numerical Integration](#47-numerical-integration)
   - [Need for Numerical Integration](#471-need-for-numerical-integration)
   - [Gaussian Quadrature (1D and 2D)](#472-gaussian-quadrature-1d-and-2d)
8. [Summary](#48-summary)

---

## 4.1 Introduction  

The efficiency and accuracy of the Finite Element Method (FEM) largely depend on the **type of element** used and the **integration scheme** applied during stiffness matrix computation.  
Finite elements are majorly classifies into several categories — **triangular**, **rectangular (quadrilateral)**, and **three-dimensional** — each having unique interpolation and geometric characteristics.  
This unit discusses these element types, the concept of **iso-parametric formulation**, and **numerical integration** (particularly Gaussian quadrature), which plays a key role in assembling global stiffness matrices.

---

## 4.2 Triangular Elements  

### 4.2.1 Linear (CST) Triangular Element  
*(Constant Strain Triangle – 3-noded element)*  

### 4.2.2 Quadratic (LST) Triangular Element  
*(Linear Strain Triangle – 6-noded element)*  

---

## 4.3 Rectangular Elements  

### 4.3.1 Bilinear Quadrilateral Element (Q4)  

### 4.3.2 Higher-Order Quadrilateral Elements  

---

## 4.4 Three-Dimensional Elements  

---

## 4.5 Iso-parametric Formulation  

### 4.5.1 Concept of Iso-parametric Elements  

### 4.5.2 Jacobian Matrix and Coordinate Transformation  

---

## 4.6 Axis-Symmetric Elements  

---

## 4.7 Numerical Integration  

### 4.7.1 Need for Numerical Integration  

In finite element formulations, a large number of **integrations** are required to evaluate element characteristic matrices such as the **stiffness matrix**, **load vector**, and **mass matrix**.  

Each term of the element stiffness matrix involves an integral over the element domain. For example, in the Galerkin formulation, integration is performed for every **interpolation (trial) function** across the physical volume or area of an element.

While a simple polynomial can be integrated analytically, the integrands that arise in FEM are often **rational functions**—ratios of polynomials. Such expressions are tedious or even impractical to integrate in closed form, especially when dealing with complex geometries or large meshes.

Hence, **finite element software** does not rely on analytical integration. Instead, it uses **numerical integration schemes**, among which **Gaussian (or Gauss–Legendre) quadrature** is the most widely adopted.

### 4.7.2 Gaussian Quadrature (1D and 2D)  

#### (a) Basic Concept  

Consider the definite integral:

$$
I = \int_{x_1}^{x_2} h(x)\,dx
$$

This integral can be transformed to a standard form over the interval $[-1, 1]$ by introducing a change of variable:

$$
r = a x + b, \quad \text{with} \quad dr = a\,dx
$$

so that

$$
I = \int_{-1}^{1} f(r)\,dr
\tag{4.7.1}
$$

In Gaussian quadrature, the integral in Eq. (4.7.1) is **approximated** by evaluating the function at specific *sampling points* (called **Gauss points**) and multiplying by *weighting factors*:

$$
I \approx \sum_{i=1}^{m} W_i\,f(r_i)
\tag{4.7.2}
$$

where  
- $r_i$ = Gauss points (sample locations),  
- $W_i$ = corresponding weights,  
- $m$ = number of integration points.

The Gauss points and weights are chosen so that the approximation is **exact** for polynomials up to a certain order.  
Specifically, for $m$ Gauss points, the integration is exact for any polynomial of order $(2m - 1)$.

Example:  
- 1-point rule → exact for a linear ($n=1$) function.  
- 2-point rule → exact for cubic ($n=3$) functions.  
- 3-point rule → exact for quintic ($n=5$) functions.

#### Accuracy of Gaussian Quadrature

For Gaussian Quadrature, the integral

$$
I = \int_{-1}^{1} f(r)\,dr \approx \sum_{i=1}^{m} W_i f(r_i)
$$

is **exact for all polynomials of order up to \( 2m - 1 \)**.

This means:

| No. of Gauss Points (m) | Polynomial Order Exactly Integrated | Example |
|--------------------------|------------------------------------|----------|
| 1-point rule             | Linear (n = 1)                     | \( f(r) = a_0 + a_1r \) |
| 2-point rule             | Cubic (n = 3)                      | \( f(r) = a_0 + a_1r + a_2r^2 + a_3r^3 \) |
| 3-point rule             | Quintic (n = 5)                    | \( f(r) = a_0 + a_1r + \dots + a_5r^5 \) |
| 4-point rule             | Septic (n = 7)                     | \( f(r) = a_0 + a_1r + \dots + a_7r^7 \) |

Hence, a **quadratic (n=2)** or **quartic (n=4)** function will also be **integrated exactly** by the 2-point and 3-point rules respectively.

#### (b) Derivation Outline  

To illustrate, consider a general polynomial:

$$
f(r) = a_0 + a_1 r + a_2 r^2 + a_3 r^3 + \cdots + a_n r^n
$$

The exact value of the integral is:

$$
\int_{-1}^{1} f(r)\,dr = 2a_0 + \frac{2}{3}a_2 + \frac{2}{5}a_4 + \cdots + \frac{2}{n+1}a_n
\tag{4.7.3}
$$

Since the integration limits are symmetric, all **odd powers** of $r$ integrate to zero.

The Gaussian approximation (Eq. 4.7.2) expands to:

$$
I \approx \sum_{i=1}^{m} W_i \big(a_0 + a_1r_i + a_2r_i^2 + a_3r_i^3 + \cdots + a_n r_i^n\big)
\tag{4.7.4}
$$

For this approximation to be **exact** for all polynomials up to order $n$, the following conditions must be satisfied:

$$
\begin{aligned}
\sum_{i=1}^{m} W_i &= 2, \\
\sum_{i=1}^{m} W_i r_i &= 0, \\
\sum_{i=1}^{m} W_i r_i^2 &= \frac{2}{3}, \\
\sum_{i=1}^{m} W_i r_i^3 &= 0, \\
\sum_{i=1}^{m} W_i r_i^4 &= \frac{2}{5}, \quad \text{and so on.}
\end{aligned}
\tag{4.7.5}
$$

These relationships can be solved to determine the Gauss points $r_i$ and the weights $W_i$.

#### (c) Illustrative Examples  

1️⃣ **For a linear polynomial ($n=1$):**  
Only the first two equations in (4.7.5) apply.  
One sampling point ($m=1$) is sufficient, giving  

$$
r_1 = 0, \quad W_1 = 2
$$

2️⃣ **For a cubic polynomial ($n=3$):**  
Three conditions apply.  
Setting $m = 2$, symmetry implies $r_1 = -r_2$, $W_1 = W_2 = 1$, which gives

$$
r_1 = -0.57735, \quad r_2 = +0.57735, \quad W_1 = W_2 = 1
$$

This corresponds to the **two-point Gauss rule**, exact for cubic polynomials.

#### (d) Gauss Points and Weights  

| No. of Points ($m$) | Sampling Points ($r_i$) | Weights ($W_i$) | Polynomial Degree Integrated Exactly |
|:-------------------:|:-----------------------:|:----------------:|:------------------------------------:|
| 1 | 0.000000 | 2.000000 | 1 |
| 2 | ±0.577350 | 1.000000 | 3 |
| 3 | 0.000000, ±0.774597 | 0.888889, 0.555556 | 5 |
| 4 | ±0.339981, ±0.861136 | 0.652145, 0.347855 | 7 |

#### (e) Two-Dimensional Gaussian Quadrature  

For double integrals, a **product rule** is applied:

$$
\int_{-1}^{1}\!\!\int_{-1}^{1} f(\xi,\eta)\, d\xi\, d\eta
\;\approx\;
\sum_{i=1}^{m}\sum_{j=1}^{m} W_i\,W_j\,f(\xi_i,\eta_j)
\tag{4.7.6}
$$

Here, $(\xi_i, \eta_j)$ are the 2D Gauss points obtained by pairing 1D Gauss points in both directions.  
This formulation is fundamental for computing **element stiffness matrices** in 2D iso-parametric elements.

#### (f) Key Points  

- Gaussian quadrature provides **efficient and accurate** evaluation of element matrices.  
- For $m$ points, it exactly integrates a polynomial of degree $(2m - 1)$.  
- It is the **default integration scheme** used in most commercial FEM software (ANSYS, ABAQUS, etc.).  
- It integrates both **shape functions** and their derivatives numerically, avoiding tedious analytical expressions.

✅ **Summary**  
- FEM requires extensive numerical integration.  
- Gaussian quadrature transforms integrals to the standard range $[-1,1]$.  
- The method uses pre-defined **Gauss points** and **weights** to achieve high accuracy with minimal computation.  
- In 2D elements, **product integration** extends the concept efficiently.

---

## 4.8 Summary  


---

## 📚 References

- [*Fundamentals of Finite Element Analysis - (Unit 3 and 4 - Chapter 6)*](Resources/FEM_Hutton_Unit3&4_Ch6.pdf) – Hutton David, McGraw-Hill 
