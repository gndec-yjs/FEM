---
title: "Unit 4 – Finite Element Types and Numerical Integration"
layout: default
mathjax: true
---

# Unit 4: Finite Element Types and Numerical Integration

## 🔗 Index
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
Hutton classifies finite elements into several categories — **triangular**, **rectangular (quadrilateral)**, and **three-dimensional** — each having unique interpolation and geometric characteristics.

This unit discusses these element types, the concept of **iso-parametric formulation**, and **numerical integration** (particularly Gaussian quadrature), which plays a key role in assembling global stiffness matrices.

---

## 4.2 Triangular Elements  

### 4.2.1 Linear (CST) Triangular Element  
*(Constant Strain Triangle – 3-noded element)*  

A **three-node triangular element** is the simplest 2-D element used in plane stress and plane strain analysis.  
Each node has **two degrees of freedom (DOFs)** — displacement in $x$ and $y$ directions ($u$, $v$).

#### Displacement Model  
The displacement field is assumed to vary **linearly** inside the element:

$$
u(x,y) = a_1 + a_2x + a_3y \\
v(x,y) = b_1 + b_2x + b_3y
$$

The strain components are constant within the element, hence the name *Constant Strain Triangle (CST)*.

#### Strain–Displacement Relation  

$$
\begin{Bmatrix}
\epsilon_x \\ \epsilon_y \\ \gamma_{xy}
\end{Bmatrix}
=
[B]
\begin{Bmatrix}
u_1 \\ v_1 \\ u_2 \\ v_2 \\ u_3 \\ v_3
\end{Bmatrix}
$$

where $[B]$ is derived from shape functions based on **area (or natural) coordinates**.

#### Stiffness Matrix  

$$
[K_e] = tA[B]^T[D][B]
$$

where  
$t =$ thickness,  
$A =$ area of the element,  
$[D] =$ material property matrix for plane stress or plane strain.

#### Features  
- Suitable for irregular geometries.  
- Simple formulation but less accurate for bending or curved boundaries.  
- Forms the basis of mesh generation in automatic meshing software.

---

### 4.2.2 Quadratic (LST) Triangular Element  
*(Linear Strain Triangle – 6-noded element)*  

The **six-noded triangular element** includes mid-side nodes to represent curvature and achieve quadratic variation of displacement.  
It improves accuracy and convergence compared to CST elements.

---

## 4.3 Rectangular Elements  

### 4.3.1 Bilinear Quadrilateral Element (Q4)  

A **4-noded rectangular element** is one of the most widely used elements in plane analysis.  
Its interpolation is *bilinear* in the local coordinates $(\xi, \eta)$.

#### Shape Functions  

$$
\begin{aligned}
N_1 &= \tfrac{1}{4}(1 - \xi)(1 - \eta) \\
N_2 &= \tfrac{1}{4}(1 + \xi)(1 - \eta) \\
N_3 &= \tfrac{1}{4}(1 + \xi)(1 + \eta) \\
N_4 &= \tfrac{1}{4}(1 - \xi)(1 + \eta)
\end{aligned}
$$

Each node has two DOFs: $u_i$, $v_i$.  
The same interpolation is used for both geometry and displacement in iso-parametric formulation.

#### Applications  
- Plane stress, plane strain, and axisymmetric problems.  
- Suitable for rectangular or nearly rectangular regions.

---

### 4.3.2 Higher-Order Quadrilateral Elements  

Higher-order elements (e.g., 8-noded or 9-noded quadrilaterals) include mid-side nodes.  
They provide quadratic displacement fields and better capture bending or stress gradients.

---

## 4.4 Three-Dimensional Elements  

### 4.4.1 Tetrahedral Element  
- 4 nodes, each with 3 DOFs ($u,v,w$).  
- Linear interpolation of displacement.  
- Suitable for complex 3-D geometries.  

### 4.4.2 Hexahedral (Brick) Element  
- 8-noded “brick” element with **trilinear interpolation**.  
- Used for regular solids like beams, pavements, or foundations.  
- Often preferred due to its higher accuracy in structured meshes.

---

## 4.5 Iso-parametric Formulation  

### 4.5.1 Concept of Iso-parametric Elements  

“Iso” means *same* — both **geometry** and **displacement field** use the same shape functions.  
Thus:

$$
x = \sum_i N_i(\xi, \eta)x_i, \quad
y = \sum_i N_i(\xi, \eta)y_i
$$

and  

$$
u = \sum_i N_i(\xi, \eta)u_i, \quad
v = \sum_i N_i(\xi, \eta)v_i
$$

Advantages:
- Handles **curved boundaries** easily.  
- Provides a consistent formulation framework for all element shapes.  
- Facilitates computer implementation via coordinate mapping.

---

### 4.5.2 Jacobian Matrix and Coordinate Transformation  

The mapping between local $(\xi, \eta)$ and global $(x, y)$ coordinates is achieved through the **Jacobian matrix**:

$$
[J] =
\begin{bmatrix}
\dfrac{\partial x}{\partial \xi} & \dfrac{\partial y}{\partial \xi} \\
\dfrac{\partial x}{\partial \eta} & \dfrac{\partial y}{\partial \eta}
\end{bmatrix}
$$

The determinant $|J|$ converts area elements:

$$
dx\,dy = |J|\,d\xi\,d\eta
$$

This transformation is essential for numerical integration of stiffness matrices.

---

## 4.6 Axis-Symmetric Elements  

Axisymmetric problems involve geometry and loading symmetric about an axis (e.g., cylinders, pipes, domes).  
Each node has displacements in the **radial** and **axial** directions only.

Strain–displacement relation in cylindrical coordinates:

$$
\begin{Bmatrix}
\epsilon_r \\ \epsilon_\theta \\ \epsilon_z \\ \gamma_{rz}
\end{Bmatrix}
=
\begin{bmatrix}
\dfrac{\partial}{\partial r} & 0 \\
\dfrac{1}{r} & 0 \\
0 & \dfrac{\partial}{\partial z} \\
\dfrac{\partial}{\partial z} & \dfrac{\partial}{\partial r}
\end{bmatrix}
\begin{Bmatrix}
u \\ v
\end{Bmatrix}
$$

---

## 4.7 Numerical Integration  

### 4.7.1 Need for Numerical Integration  
In iso-parametric and higher-order elements, analytical integration of stiffness matrices becomes difficult.  
**Numerical integration (quadrature)** techniques approximate the integral by summing weighted function values at specific points.

### 4.7.2 Gaussian Quadrature (1D and 2D)  

For a 1D integral over $[-1,1]$:

$$
\int_{-1}^{1} f(\xi)d\xi \approx \sum_{i=1}^{n} w_i f(\xi_i)
$$

| Order | Gauss Points $(\xi_i)$ | Weights $(w_i)$ |
|:------:|:--------------------:|:----------------:|
| 1 | 0 | 2 |
| 2 | ±0.57735 | 1 |
| 3 | 0, ±0.774597 | 0.8889, 0.5556 |

For 2-D quadrature, product integration is used:

$$
\int_{-1}^{1}\int_{-1}^{1} f(\xi,\eta)\,d\xi\,d\eta \approx \sum_{i=1}^{n}\sum_{j=1}^{n} w_i w_j f(\xi_i,\eta_j)
$$

---

## 4.8 Summary  

- **Triangular**, **rectangular**, and **3-D elements** represent the main finite element geometries.  
- **Iso-parametric formulation** uses the same interpolation for geometry and displacement.  
- **Axisymmetric elements** simplify rotationally symmetric analyses.  
- **Gaussian quadrature** is an efficient tool for numerical integration of element matrices.

---
