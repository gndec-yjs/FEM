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

## Unit 2: Beam Elements

- [Flexure Element](#flexure-elements)  
- [Element Stiffness Matrix](#flexure-element-stiffness-matrix)  
- [Element Load Vector](#element-load-vector)  

---

# Flexure Elements

## Introduction

One-dimensional axial load-only elements are useful for analyzing simple structures. However, they **cannot transmit bending effects**, limiting their application in commonly encountered structures with welded or riveted joints.

To address this, **elementary beam theory** is applied to develop a **flexure (beam) element** capable of representing **transverse bending effects**.

* Initially, the element is presented as a **1D line element** capable of bending in a plane.
* Discretized equations are developed using **polynomial interpolation functions**.
* Formulation can be extended to **two-plane bending**, and **axial loading and torsion** effects can be included.

---

## Elementary Beam Theory

### Beam under Distributed Load

Consider a simply supported beam subjected to a distributed transverse load $q(x)$, expressed as force per unit length.

**Coordinate system:**

* $x$ → axial direction
* $y$ → transverse direction

**Assumptions:**

1. Beam loaded only in the $y$-direction.
2. Small deflections relative to beam dimensions.
3. Material is linearly elastic, isotropic, and homogeneous.
4. Beam is prismatic, with a cross-section symmetric about the plane of bending.

<img width="866" height="429" alt="image" src="https://github.com/user-attachments/assets/08e0608e-b8a1-4378-8359-ccd7323dd0b1" />

**Figure:** Simply supported beam under distributed load

### Cross-Section Symmetry

* Symmetric sections (rectangular, triangular) bend **in-plane**.
* Asymmetric sections (L-shaped) bend **out-of-plane**.
* Maximum deflection $\delta_{\max} < 0.1h$, where $h$ = cross-section depth.


<img width="660" height="373" alt="image" src="https://github.com/user-attachments/assets/cdf4ff25-891e-4021-8f25-e7c3a0e2c0f5" />

**Figure:** Beam cross-sections (symmetric vs. asymmetric)


### Neutral Surface and Bending Strain

* Top surface shortens, bottom elongates → **neutral surface** at $y=0$.
* Differential length after bending:

$$
ds = (1 - y / \rho) dx
$$

* Bending strain:

$$
\varepsilon_x = \frac{ds - dx}{dx} = -y \frac{1}{\rho}
$$

* For small deflections:

$$
\frac{1}{\rho} \approx \frac{d^2 v}{dx^2} \implies \varepsilon_x = -y \frac{d^2 v}{dx^2}
$$

**Normal stress:**

$$
\sigma_x = E \varepsilon_x = -E y \frac{d^2 v}{dx^2}
$$

* Stress varies **linearly with distance from the neutral surface**.
* Neutral surface passes through **centroid**:

$$
\int_A y \, dA = 0
$$

### Bending Moment

* Internal bending moment:

$$
M(x) = \int_A y \sigma_x \, dA = E I_z \frac{d^2 v}{dx^2}
$$

where $I_z = \int_A y^2 dA$ = moment of inertia.

* Normal stress in terms of bending moment:

$$
\sigma_x = -\frac{M(x) y}{I_z}
$$

---

## Flexure Element

### Assumptions for Flexure Element

1. Length $L$ with **two nodes**.
2. Connected to other elements **only at nodes**.
3. Loading occurs **only at nodes**.

* Field variable: transverse displacement $v(x)$ of neutral surface.
* Both **displacement** and **slope (rotation)** at nodes are considered.

### Nodal Variables

* Nodes 1 and 2 at element ends.
* Variables: $v_1, v_2$ (displacements), $\theta_1, \theta_2$ (slopes in radians).
* Boundary conditions:

$$
\begin{align}
v(x_1) &= v_1, & v(x_2) &= v_2 \\
\frac{dv}{dx}\big|_{x=x_1} &= \theta_1, & \frac{dv}{dx}\big|_{x=x_2} &= \theta_2
\end{align}
$$

<img width="878" height="575" alt="image" src="https://github.com/user-attachments/assets/87b47b3a-7a18-4f39-961d-336f8bb554f2" />

**Figure:** Beam element nodal variables and Beam element nodal displacements shown in a positive sense


### Displacement Function

* Use a **cubic polynomial**:

$$
v(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3
$$

* Apply boundary conditions to solve coefficients:

$$
\begin{align}
a_0 &= v_1 \\
a_1 &= \theta_1 \\
a_2 &= \frac{3}{L^2} (v_2 - v_1) - \frac{1}{L}(2\theta_1 + \theta_2) \\
a_3 &= \frac{2}{L^3}(v_1 - v_2) + \frac{1}{L^2}(\theta_1 + \theta_2)
\end{align}
$$

* Displacement in **shape function form**:

$$
v(x) = N_1(x)v_1 + N_2(x)\theta_1 + N_3(x)v_2 + N_4(x)\theta_2
$$

* $N_1, N_2, N_3, N_4$ = **interpolation functions**.
* Dimensionless coordinate: $\xi = x/L$.

<img width="599" height="612" alt="image" src="https://github.com/user-attachments/assets/955af7ed-453b-4029-90f2-9ae215a1e812" />

**Figure:** Shape functions along element length

### Stress Computation

* Normal stress at cross-section:

$$
\sigma_x(x, y) = -y E \frac{d^2[N]\{v_1, \theta_1, v_2, \theta_2\}}{dx^2}
$$

* Maximum stress occurs at outer fibers ($y_{\max}$):

$$
\sigma_x(x) = y_{\max} E \frac{d^2[N]\{v_1, \theta_1, v_2, \theta_2\}}{dx^2}
$$

* Nodal stresses:

$$
\begin{align}
\sigma_x(0) &= \frac{6 y_{\max} E}{L^2} (v_2 - v_1) - \frac{2 y_{\max} E}{L} (2\theta_1 + \theta_2) \\
\sigma_x(L) &= \frac{6 y_{\max} E}{L^2} (v_1 - v_2) + \frac{2 y_{\max} E}{L} (2\theta_2 + \theta_1)
\end{align}
$$

* Stress varies **linearly along the element**, so only **nodal stresses** need calculation after nodal displacements are known.

**Figure 6:** Stress distribution along a flexure element
*(Placeholder: linear stress distribution along the beam with max tension/compression)*

---

# Flexure Element Stiffness Matrix

Having developed the **flexure element displacement approximation**, we now proceed to determine the **stiffness characteristics** of the element. The stiffness matrix establishes the relationship between nodal displacements and nodal forces, which is fundamental in finite element analysis.

## Strain Energy of the Flexure Element

The total **strain energy** stored in a beam element due to bending can be expressed as:

$$
U_e = \frac{1}{2} \int_V \sigma_x \varepsilon_x \, dV
$$

where $V$ represents the total volume of the element, $\sigma_x$ is the longitudinal stress, and $\varepsilon_x$ is the corresponding strain.

Substituting the expressions for stress and strain obtained earlier:

$$
\varepsilon_x = -y \frac{d^2 v}{dx^2}, \quad \sigma_x = E \varepsilon_x = -E y \frac{d^2 v}{dx^2}
$$

yields:

$$
U_e = \frac{E}{2} \int_V y^2 \left(\frac{d^2 v}{dx^2}\right)^2 dV
$$

For a beam with constant cross-section, the volume integral can be separated into an area and a length integral:

$$
U_e = \frac{E}{2} \int_0^L \left(\frac{d^2 v}{dx^2}\right)^2 \left(\int_A y^2 dA \right) dx
$$

The area integral is recognized as the **moment of inertia $I_z$** about the centroidal axis perpendicular to the bending plane, giving:

$$
U_e = \frac{E I_z}{2} \int_0^L \left(\frac{d^2 v}{dx^2}\right)^2 dx
$$

This expression represents the **strain energy of bending** for a beam element that satisfies the assumptions of elementary beam theory.

## Discretized Strain Energy Using Nodal Variables

Using the **finite element approximation** for transverse displacement:

$$
v(x) = N_1(x)v_1 + N_2(x)\theta_1 + N_3(x)v_2 + N_4(x)\theta_2
$$

the strain energy can be written in terms of nodal variables as:

$$
U_e = \frac{E I_z}{2} \int_0^L \left( \frac{d^2 N_1}{dx^2} v_1 + \frac{d^2 N_2}{dx^2} \theta_1 + \frac{d^2 N_3}{dx^2} v_2 + \frac{d^2 N_4}{dx^2} \theta_2 \right)^2 dx
$$

This integral is an **approximation**, as the discretized displacement function is generally not the exact solution for the beam bending problem. However, it provides a sufficiently accurate representation for finite element analysis.

## Nodal Forces and Moments (Castigliano’s Theorem)

By applying **Castigliano’s first theorem** to the strain energy, the **nodal forces and moments** can be obtained by differentiating $U_e$ with respect to the corresponding nodal displacement:

$$
F_1 = \frac{\partial U_e}{\partial v_1}, \quad M_1 = \frac{\partial U_e}{\partial \theta_1}, \quad F_2 = \frac{\partial U_e}{\partial v_2}, \quad M_2 = \frac{\partial U_e}{\partial \theta_2}
$$

Substituting the discretized displacement function:

$$
\begin{aligned}
F_1 &= E I_z \int_0^L \left( \sum_{i=1}^4 \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_1}{dx^2} dx, \\
M_1 &= E I_z \int_0^L \left( \sum_{i=1}^4 \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_2}{dx^2} dx, \\
F_2 &= E I_z \int_0^L \left( \sum_{i=1}^4 \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_3}{dx^2} dx, \\
M_2 &= E I_z \int_0^L \left( \sum_{i=1}^4 \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_4}{dx^2} dx,
\end{aligned}
$$

where $q_i = \{v_1, \theta_1, v_2, \theta_2\}$.

## Element Stiffness Matrix

These equations relate **nodal displacements** to **nodal forces** in the standard form:

$$
\begin{bmatrix}
k_{11} & k_{12} & k_{13} & k_{14} \\
k_{21} & k_{22} & k_{23} & k_{24} \\
k_{31} & k_{32} & k_{33} & k_{34} \\
k_{41} & k_{42} & k_{43} & k_{44}
\end{bmatrix}
\begin{bmatrix}
v_1 \\ \theta_1 \\ v_2 \\ \theta_2
\end{bmatrix}
=
\begin{bmatrix}
F_1 \\ M_1 \\ F_2 \\ M_2
\end{bmatrix}
$$

with the **stiffness coefficients**:

$$
k_{mn} = E I_z \int_0^L \frac{d^2 N_m}{dx^2} \frac{d^2 N_n}{dx^2} dx, \quad m,n=1,4
$$

### Transformation to Dimensionless Coordinate

To simplify integration, introduce the **dimensionless coordinate**:

$$
\xi = \frac{x}{L} \quad \Rightarrow \quad dx = L \, d\xi, \quad \frac{d}{dx} = \frac{1}{L} \frac{d}{d\xi}
$$

The stiffness coefficients become:

$$
k_{mn} = \frac{E I_z}{L^3} \int_0^1 \frac{d^2 N_m}{d\xi^2} \frac{d^2 N_n}{d\xi^2} d\xi
$$

### Computed Stiffness Coefficients

Direct integration yields:

$$
\begin{aligned}
k_{11} &= 12 \frac{E I_z}{L^3}, & k_{12} &= 6 \frac{E I_z}{L^2}, & k_{13} &= -12 \frac{E I_z}{L^3}, & k_{14} &= 6 \frac{E I_z}{L^2}, \\
k_{22} &= 4 \frac{E I_z}{L}, & k_{23} &= -6 \frac{E I_z}{L^2}, & k_{24} &= 2 \frac{E_z}{L}, \\
k_{33} &= 12 \frac{E I_z}{L^3}, & k_{34} &= -6 \frac{E I_z}{L^2}, & k_{44} &= 4 \frac{E I_z}{L}
\end{aligned}
$$

The **stiffness matrix** is symmetric: $k_{mn} = k_{nm}$.

### Final Flexure Element Stiffness Matrix

$$
[k_e] = \frac{E I_z}{L^3} 
\begin{bmatrix}
12 & 6L & -12 & 6L \\
6L & 4L^2 & -6L & 2L^2 \\
-12 & -6L & 12 & -6L \\
6L & 2L^2 & -6L & 4L^2
\end{bmatrix}
$$

**Remarks:**

* Valid for any consistent unit system.
* Rotational DOFs ($\theta_1, \theta_2$) must be expressed in **radians**.
* The matrix is singular if the element is unconstrained due to **rigid body motion**.

This concludes the derivation of the **flexure element stiffness matrix**, linking nodal displacements and applied forces/moments for bending analysis in finite element modeling.

---
## 📚 Reference

- [*Fundamentals of Finite Element Analysis - (Unit 2 - Chapter 4)*](Resources/FEM_Hutton_Unit2_Ch4.pdf) – Hutton David, McGraw-Hill 







