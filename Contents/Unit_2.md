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
- [Flexure Element Stiffness Matrix](#flexure-element-stiffness-matrix)  
- [Element Load Vector](#element-load-vector)  

# Flexure Elements 

## 4.1 Introduction

The one-dimensional, axial load-only elements discussed in earlier chapters are useful for analyzing many simple structures. However, they **cannot transmit bending effects**, which limits their application in structures with welded or riveted joints.

Elementary beam theory is applied here to develop a **flexure (beam) element** capable of properly representing **transverse bending effects**.

* The element is first presented as a **1D line element** capable of bending in a plane.
* Discretized equations are developed using **polynomial interpolation functions**.
* Later, the formulation can be extended to **two-plane bending**, **axial loading**, and **torsion**.

## 4.2 Elementary Beam Theory

Consider a **simply supported beam** subjected to a **distributed transverse load** $q(x)$, expressed in force per unit length.

*Coordinate system:*

* $x$ → axial coordinate
* $y$ → transverse coordinate

**Assumptions:**

1. Beam loaded only in $y$-direction.
2. Small deflections relative to beam dimensions.
3. Material is linearly elastic, isotropic, homogeneous.
4. Beam is prismatic and the cross-section has an axis of symmetry in the plane of bending.

<img width="866" height="429" alt="image" src="https://github.com/user-attachments/assets/08e0608e-b8a1-4378-8359-ccd7323dd0b1" />

**Figure 4.1:** Simply supported beam under arbitrary distributed load
*(a) Load, (b) deflected shape, (c) sign convention for shear force and bending moment)*


### Cross-Section Symmetry

* Symmetric sections (rectangular, triangular) bend **in-plane**.
* Asymmetric sections (L-shaped) bend **out-of-plane**.
* Maximum deflection $\delta_{\max} < 0.1h$, where $h$ = cross-section depth.

<img width="660" height="373" alt="image" src="https://github.com/user-attachments/assets/cdf4ff25-891e-4021-8f25-e7c3a0e2c0f5" />

**Figure 4.2:** Beam cross sections:
*(a) and (b) symmetric sections, (c) asymmetric L-shaped section*

## Neutral Surface and Bending Strain

Consider a differential fiber of a beam after bending, as shown in Figure 4.1b of Hutton. Let the **neutral surface** be located at $y=0$, the distance from the center of curvature be $\rho$, and the angular increment along the arc be $d\theta$.

**Length of a fiber at distance $y$ from the neutral surface:**

$$
ds = (\rho - y) d\theta \tag{4.1}
$$

**Bending strain** along the fiber:

$$
\varepsilon_x = \frac{ds - dx}{dx} = \frac{(\rho - y)d\theta - \rho d\theta}{\rho d\theta} = -\frac{y}{\rho} \tag{4.2}
$$

### Radius of Curvature

From basic calculus, the radius of curvature of a planar curve $v=v(x)$ is:

$$
\frac{1}{\rho} = \frac{d^2 v/dx^2}{\left(1 + (dv/dx)^2\right)^{3/2}} \tag{4.3}
$$

For **small deflections** (slopes $dv/dx \ll 1$):

$$
\frac{1}{\rho} \approx \frac{d^2 v}{dx^2} \tag{4.4}
$$

### Normal Strain and Stress

Using this approximation, the **normal strain along the longitudinal axis** is:

$$
\varepsilon_x = -y \frac{d^2 v}{dx^2} \tag{4.5}
$$

The corresponding **normal stress** is:

$$
\sigma_x = E \varepsilon_x = -E y \frac{d^2 v}{dx^2} \tag{4.6}
$$

Since there is no net axial force on the cross-section:

$$
F_x = \int_A \sigma_x \, dA = - \int_A E y \frac{d^2 v}{dx^2} \, dA = 0 \tag{4.7}
$$

This confirms that the **neutral surface passes through the centroid**:

$$
\int_A y \, dA = 0 \tag{4.8}
$$

### Bending Moment

The **internal bending moment** at a cross-section:

$$
M(x) = - \int_A y \sigma_x \, dA = E \int_A y^2 \frac{d^2 v}{dx^2} \, dA \tag{4.9}
$$

Using the **moment of inertia**:

$$
I_z = \int_A y^2 dA
$$

The bending moment becomes:

$$
M(x) = E I_z \frac{d^2 v}{dx^2} \tag{4.10}
$$

The **normal stress in terms of bending moment**:

$$
\sigma_x = -\frac{M(x) y}{I_z} = -y E \frac{d^2 v}{dx^2} \tag{4.11}
$$


## 4.3 Flexure Element

**Assumptions:**

1. Length $L$ with **two nodes**.
2. Connected to other elements **only at nodes**.
3. Loading occurs **only at nodes**.

Field variable: transverse displacement $v(x)$ of the neutral surface.

### Nodal Variables

* Nodes 1 and 2 at element ends.
* Variables: $v_1, v_2$ (displacements), $\theta_1, \theta_2$ (slopes/rotations in radians).

Boundary conditions:

$$
v(x_1) = v_1, \quad v(x_2) = v_2 \tag{4.12}
$$

$$
\frac{dv}{dx}\Big|_{x=x_1} = \theta_1, \quad \frac{dv}{dx}\Big|_{x=x_2} = \theta_2 \tag{4.13}
$$

Or equivalently:

$$
v(0) = v_1, \quad v(L) = v_2 \tag{4.14}
$$

$$
\frac{dv}{dx}(0) = \theta_1, \quad \frac{dv}{dx}(L) = \theta_2 \tag{4.15, 4.16}
$$

<img width="878" height="575" alt="image" src="https://github.com/user-attachments/assets/87b47b3a-7a18-4f39-961d-336f8bb554f2" />

**Figure:** Beam element nodal variables and Beam element nodal displacements shown in a positive sense

---

### Displacement Function

Assume a cubic polynomial form for transverse displacement:

$$
v(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3 \tag{4.17}
$$

#### Applying Boundary Conditions

From finite element formulation of the beam (flexure element), we apply the boundary conditions at nodes:

1. At $x = 0$, displacement:
   $$
   v(x=0) = v_1 = a_0 \tag{4.18}
   $$

2. At $x = L$, displacement:
   $$
   v(x=L) = v_2 = a_0 + a_1 L + a_2 L^2 + a_3 L^3 \tag{4.19}
   $$

3. At $x = 0$, slope:
   $$
   \left.\frac{dv}{dx}\right|_{x=0} = \theta_1 = a_1 \tag{4.20}
   $$

4. At $x = L$, slope:
   $$
   \left.\frac{dv}{dx}\right|_{x=L} = \theta_2 = a_1 + 2a_2 L + 3a_3 L^2 \tag{4.21}
   $$

<img width="599" height="612" alt="image" src="https://github.com/user-attachments/assets/955af7ed-453b-4029-90f2-9ae215a1e812" />

**Figure 4.5:** Bending moment diagram for a flexure element

#### Solving for Coefficients

Equations (4.18)–(4.21) are solved simultaneously to obtain:

$$
a_0 = v_1 \tag{4.22}
$$

$$
a_1 = \theta_1 \tag{4.23}
$$

$$
a_2 = \frac{3}{L^2}(v_2 - v_1) - \frac{1}{L}(2\theta_1 + \theta_2) \tag{4.24}
$$

$$
a_3 = \frac{2}{L^3}(v_1 - v_2) + \frac{1}{L^2}(\theta_1 + \theta_2) \tag{4.25}
$$

#### Final Displacement Expression

Substituting into $v(x)$:

$$
v(x) = \left(1 - 3\frac{x^2}{L^2} + 2 \frac{x^3}{L^3}\right)v_1 + \left(x - 2\frac{x^2}{L} + \frac{x^3}{L^2}\right)\theta_1 + \left(3\frac{x^2}{L^2} - 2\frac{x^3}{L^3}\right)v_2 + \left(-\frac{x^2}{L} + \frac{x^3}{L^2}\right)\theta_2 \tag{4.26}
$$

#### which is of the form

$$
v(x) = N_1(x)v_1 + N_2(x)\theta_1 + N_3(x)v_2 + N_4(x)\theta_2 \tag{4.27a}
$$

#### Matrix Form

$$
v(x) =
\begin{bmatrix}
N_1 & N_2 & N_3 & N_4
\end{bmatrix}
\begin{Bmatrix}
v_1 \\
\theta_1 \\
v_2 \\
\theta_2
\end{Bmatrix}
= [N]\{v\} \tag{4.27b}
$$

#### Using Dimensionless Coordinate

Introduce $\xi = \dfrac{x}{L}$:

$$
v(x) = (1 - 3\xi^2 + 2\xi^3)v_1 + L(\xi - 2\xi^2 + \xi^3)\theta_1 + (3\xi^2 - 2\xi^3)v_2 + L(-\xi^2 + \xi^3)\theta_2 \tag{4.29}
$$

_where 0 ≤ ξ ≤ 1. This form proves more amenable to the integrations required to complete development of the element equations in the next section._


### Stress Computation

Using Equation (4.11) and interpolation:

$$
\sigma_x(x,y) = -y E \frac{d^2 [N]}{dx^2} \{\delta\} \tag{4.30}
$$

Maximum stress occurs at outer fibers $y_{\max}$:

$$
\sigma_x(x) = y_{\max} E \frac{d^2 [N]}{dx^2} \{\delta\} \tag{4.31}
$$

Expanded form:

$$
\sigma_x(x) = y_{\max} E \Bigg[ 
\left(\frac{12x}{L^3} - \frac{6}{L^2}\right) v_1 +
\left(\frac{6x}{L^2} - \frac{4}{L}\right) \theta_1 +
\left(-\frac{12x}{L^3} + \frac{6}{L^2}\right) v_2 +
\left(\frac{6x}{L^2} - \frac{2}{L}\right) \theta_2
\Bigg] \tag{4.32}
$$

Nodal stresses:

$$
\sigma_x(x=0) = y_{\max} E \left[ \frac{6}{L^2}(v_2 - v_1) - \frac{2}{L}(2\theta_1 + \theta_2) \right] \tag{4.33}
$$

$$
\sigma_x(x=L) = y_{\max} E \left[ \frac{6}{L^2}(v_1 - v_2) + \frac{2}{L}(2\theta_2 + \theta_1) \right] \tag{4.34}
$$


---

# Flexure Element Stiffness Matrix

Having developed the **flexure element displacement approximation**, we now examine the **stiffness characteristics** of the element, which relate **nodal displacements** to **nodal forces**.

## Strain Energy of the Flexure Element

The total **strain energy** stored in the element under bending is expressed as:

$$
U_e = \frac{1}{2} \int_V \sigma_x \varepsilon_x \, dV \tag{4.35}
$$

where $V$ is the total volume of the element.

Substituting the expressions for stress and strain from Equations 4.5 and 4.6:

$$
\varepsilon_x = -y \frac{d^2 v}{dx^2}, \quad \sigma_x = E \varepsilon_x = -E y \frac{d^2 v}{dx^2} \tag{4.36}
$$

yields:

$$
U_e = \frac{E}{2} \int_V y^2 \left(\frac{d^2 v}{dx^2}\right)^2 dV
$$

For a constant cross-section, the integral can be separated:

$$
U_e = \frac{E}{2} \int_0^L \left(\frac{d^2 v}{dx^2}\right)^2 \left( \int_A y^2 \, dA \right) dx \tag{4.37}
$$

Recognizing the area integral as the **moment of inertia $I_z$**:

$$
U_e = \frac{E I_z}{2} \int_0^L \left(\frac{d^2 v}{dx^2}\right)^2 dx \tag{4.38}
$$

## Discretized Strain Energy Using Nodal Variables

Using the finite element approximation (Equation 4.27):

$$
v(x) = N_1(x)v_1 + N_2(x)\theta_1 + N_3(x)v_2 + N_4(x)\theta_2
$$

the strain energy becomes:

$$
U_e = \frac{E I_z}{2} \int_0^L \left( \frac{d^2 N_1}{dx^2} v_1 + \frac{d^2 N_2}{dx^2} \theta_1 + \frac{d^2 N_3}{dx^2} v_2 + \frac{d^2 N_4}{dx^2} \theta_2 \right)^2 dx \tag{4.39}
$$

This is an **approximation**, since the discretized displacement is generally not the exact solution.


## Nodal Forces and Moments (Castigliano’s Theorem)

By differentiating $U_e$ with respect to nodal displacements, we obtain the **nodal forces and moments**:

$$
F_1 = \frac{\partial U_e}{\partial v_1} = E I_z \int_0^L \left( \sum_{i=1}^{4} \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_1}{dx^2} dx \tag{4.40}
$$

$$
M_1 = \frac{\partial U_e}{\partial \theta_1} = E I_z \int_0^L \left( \sum_{i=1}^{4} \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_2}{dx^2} dx \tag{4.41}
$$

$$
F_2 = \frac{\partial U_e}{\partial v_2} = E I_z \int_0^L \left( \sum_{i=1}^{4} \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_3}{dx^2} dx \tag{4.42}
$$

$$
M_2 = \frac{\partial U_e}{\partial \theta_2} = E I_z \int_0^L \left( \sum_{i=1}^{4} \frac{d^2 N_i}{dx^2} q_i \right) \frac{d^2 N_4}{dx^2} dx \tag{4.43}
$$

where $q_i = \{v_1, \theta_1, v_2, \theta_2\}$.

## Element Stiffness Matrix

The nodal forces and moments can be written in matrix form:

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
\end{bmatrix} \tag{4.44}
$$

with the **stiffness coefficients**:

$$
k_{mn} = k_{nm} = E I_z \int_0^L \frac{d^2 N_m}{dx^2} \frac{d^2 N_n}{dx^2} dx, \quad m,n=1,4 \tag{4.45}
$$

## Transformation to Dimensionless Coordinate

Introduce the dimensionless variable:

$$
\xi = \frac{x}{L} \quad \Rightarrow \quad dx = L \, d\xi, \quad \frac{d}{dx} = \frac{1}{L} \frac{d}{d\xi} \tag{4.46, 4.47}
$$

Then the stiffness coefficients become:

$$
k_{mn} = k_{nm} = \frac{E I_z}{L^3} \int_0^1 \frac{d^2 N_m}{d\xi^2} \frac{d^2 N_n}{d\xi^2} d\xi \tag{4.48}
$$

## Computed Stiffness Coefficients

Direct integration yields:

$$
\begin{aligned}
k_{11} &= 12 \frac{E I_z}{L^3}, & k_{12} &= 6 \frac{E I_z}{L^2}, & k_{13} &= -12 \frac{E I_z}{L^3}, & k_{14} &= 6 \frac{E I_z}{L^2}, \\
k_{22} &= 4 \frac{E I_z}{L}, & k_{23} &= -6 \frac{E I_z}{L^2}, & k_{24} &= 2 \frac{E I_z}{L}, \\
k_{33} &= 12 \frac{E I_z}{L^3}, & k_{34} &= -6 \frac{E I_z}{L^2}, & k_{44} &= 4 \frac{E I_z}{L}
\end{aligned}
$$

The matrix is symmetric: $k_{mn} = k_{nm}$.

## Final Flexure Element Stiffness Matrix

$$
[k_e] = \frac{E I_z}{L^3} 
\begin{bmatrix}
12 & 6L & -12 & 6L \\
6L & 4L^2 & -6L & 2L^2 \\
-12 & -6L & 12 & -6L \\
6L & 2L^2 & -6L & 4L^2
\end{bmatrix} \tag{4.49}
$$

**Remarks:**

* Rotational DOFs ($\theta_1, \theta_2$) must be in **radians**.
* The matrix is singular if the element is unconstrained due to **rigid body motion**.
* Valid for any consistent unit system.

---

# Element Load Vector

In Equations 4.40–4.43, the **element forces and moments** were treated as per **Castigliano’s first theorem**, acting in the direction of the associated nodal displacements. These directions are consistent with the **assumed positive directions** of the nodal displacements.

However, the usual **beam theory sign conventions** for shear force and bending moment differ, as shown in Figures 4.6a and 4.6b. This can be expressed as:

$$
\begin{bmatrix}
F_1 \\ M_1 \\ F_2 \\ M_2
\end{bmatrix}_{\text{FE}} 
\quad \Rightarrow \quad
\begin{bmatrix}
- V_1 \\ - M_1 \\ V_2 \\ M_2
\end{bmatrix}_{\text{beam theory}} \tag{4.50}
$$

Here:

* The **left-hand side** represents positive nodal forces and moments according to finite element formulations.
* The **right-hand side** contains the corresponding **signed shear forces and bending moments** per the **beam theory convention**.

## Nodal Forces at Element Junctions

<img width="901" height="341" alt="image" src="https://github.com/user-attachments/assets/446378bc-2278-46dc-973c-c4f68dbc1664" />

* **Figure 4.6:** (a) Nodal load positive convention. (b) Positive convention from the strength of materials theory. (c) Shear and bending moment diagrams depicting nodal load effects

If two flexure elements share a **common node**, the internal shear forces are **equal and opposite**, unless an **external force** is applied at that node. In such a case, the sum of the internal shear forces equals the applied load.

Similarly, for **bending moments**:

* Internal bending moments at a node are **equal and opposite**, self-equilibrating, unless a **concentrated moment** is applied.
* In that event, the internal moments sum to the **applied external moment**.

These observations are illustrated in **Figure 4.6c**, which shows a simply supported beam with:

* A **concentrated force** at the midpoint
* A **concentrated bending moment** at the midpoint

The **shear force diagram** exhibits a **jump discontinuity** at the point of application of the concentrated force. The magnitude of this jump equals the magnitude of the applied force.

Similarly, the **bending moment diagram** shows a **jump discontinuity** equal to the magnitude of the applied bending moment.

## Element Load Vector in Finite Element Assembly

If the beam is divided into **two finite elements** with a connecting node at the midpoint:

* The **net force** at the connecting node is equal to the **applied external force**.
* The **net moment** at the connecting node is equal to the **applied external moment**.

This ensures that the finite element model accurately incorporates **external nodal loads** while maintaining equilibrium at shared nodes.

---

## Example 4.1: Midspan Deflection of a Statically Indeterminate Beam

**Problem:**
A statically indeterminate simply supported beam as shown in figure 4.7a  is subjected to a **transverse load** applied at midspan. Using **two flexure elements**, obtain the solution for the **midspan deflection**.

**Solution:**

Since **flexure elements require loading only at nodes**, the beam is divided into **two elements**, each of length $L/2$, as shown in **Figure 4.7b**.

<img width="850" height="450" alt="image" src="https://github.com/user-attachments/assets/2b5672e5-65ec-4efe-8004-167d28cc7a4c" />


### Step 1: Element Stiffness Matrices

The individual element stiffness matrices are:

$$
k^{(1)} = k^{(2)} = \frac{E I_z}{(L/2)^3} 
\begin{bmatrix}
12 & 6(L/2) & -12 & 6(L/2) \\
6(L/2) & 4(L/2)^2 & -6(L/2) & 2(L/2)^2 \\
-12 & -6(L/2) & 12 & -6(L/2) \\
6(L/2) & 2(L/2)^2 & -6(L/2) & 4(L/2)^2
\end{bmatrix}
$$

Simplifying:

$$
k^{(1)} = k^{(2)} = \frac{8 E I_z}{L^3} 
\begin{bmatrix}
12 & 3L & -12 & 3L \\
3L & L^2 & -3L & L^2/2 \\
-12 & -3L & 12 & -3L \\
3L & L^2/2 & -3L & L^2
\end{bmatrix}
$$

**Note:** The length of each element is $L/2$.

### Step 2: Displacement Correspondence

Boundary conditions:

$$
v_1 = \theta_1 = v_3 = 0
$$

Element-to-system displacement correspondence is given in **Table 4.1**:

| Global Displacement | Element 1 | Element 2 |
| ------------------- | --------- | --------- |
| 1                   | 1         | 0         |
| 2                   | 2         | 0         |
| 3                   | 3         | 1         |
| 4                   | 4         | 2         |
| 5                   | 0         | 3         |
| 6                   | 0         | 4         |

### Step 3: Assembling the Global Stiffness Matrix

Using the displacement correspondence and **symmetry**, the global stiffness coefficients are:

$$
\begin{aligned}
K_{11} &= k^{(1)}_{11} = \frac{96 E I_z}{L^3}, & K_{12} &= k^{(1)}_{12} = \frac{24 E I_z}{L^2}, & K_{13} &= k^{(1)}_{13} = -\frac{96 E I_z}{L^3}, & K_{14} &= k^{(1)}_{14} = \frac{24 E I_z}{L^2} \\
K_{22} &= k^{(1)}_{22} = \frac{8 E I_z}{L}, & K_{23} &= k^{(1)}_{23} = -\frac{24 E I_z}{L^2}, & K_{24} &= k^{(1)}_{24} = \frac{4 E I_z}{L} \\
K_{33} &= k^{(1)}_{33} + k^{(2)}_{11} = \frac{192 E I_z}{L^3}, & K_{34} &= k^{(1)}_{34} + k^{(2)}_{12} = 0, & K_{35} &= k^{(2)}_{13} = -\frac{96 E I_z}{L^3}, & K_{36} = k^{(2)}_{14} = \frac{24 E I_z}{L^2} \\
K_{44} &= k^{(1)}_{44} + k^{(2)}_{22} = \frac{16 E I_z}{L}, & K_{45} &= k^{(2)}_{23} = -\frac{24 E I_z}{L^2}, & K_{46} = k^{(2)}_{24} = \frac{4 E I_z}{L} \\
K_{55} &= k^{(2)}_{33} = \frac{96 E I_z}{L^3}, & K_{56} &= k^{(2)}_{34} = -\frac{24 E I_z}{L^2}, & K_{66} = k^{(2)}_{44} = \frac{8 E I_z}{L}
\end{aligned}
$$

Thus, the global system is written as:

$$
[K]\{U\} = \{F\} 
$$

$$
\frac{E I_z}{L^3}
\begin{bmatrix}
96 & 24L & -96 & 24L & 0 & 0 \\
24L & 8L^2 & -24L & 4L^2 & 0 & 0 \\
-96 & -24L & 192 & 0 & -96 & 24L \\
24L & 4L^2 & 0 & 16L^2 & -24L & 4L^2 \\
0 & 0 & -96 & -24L & 96 & 24L \\
0 & 0 & 24L & 4L^2 & 24L & 8L^2
\end{bmatrix}
\begin{Bmatrix}
v_1 \\ \theta_1 \\ v_2 \\ \theta_2 \\ v_3 \\ \theta_3
\end{Bmatrix}
=
\begin{Bmatrix}
F_1 \\ M_1 \\ F_2 \\ M_2 \\ F_3 \\ M_3
\end{Bmatrix}
$$

### Step 4: Apply Boundary Conditions

Using $v_1 = \theta_1 = v_3 = 0$, the **reduced system** becomes:

$$
\frac{E I_z}{L^3}
\begin{bmatrix}
192 & 0 & 24L \\
0 & 16 L^2 & 4L^2 \\
24L & 4L^2 & 8 L^2
\end{bmatrix}
\begin{Bmatrix}
v_2 \\ \theta_2 \\ \theta_3
\end{Bmatrix}
=
\begin{Bmatrix}
- P \\ 0 \\ 0
\end{Bmatrix}
$$

### Step 5: Solve for Nodal Displacements

$$
v_2 = - \frac{7 P L^3}{768 E I_z} = - \frac{P L^2}{128 E I_z}, \quad 
\theta_2 = 2 = \dots, \quad 
v_3 = \frac{P L^2}{32 E I_z}
$$

The deformed shape of the beam is shown in **Figure 4.7c**, superimposed with the **undeformed shape**.


### Step 6: Compute Reactions

Substituting nodal displacements into constraint equations:

$$
\begin{aligned}
F_1 &= \frac{E I_z}{L^3}(-96 v_2 + 24 L \theta_2) = \frac{11 P}{16} \\
F_3 &= \frac{E I_z}{L^3}(-96 v_2 - 24 L \theta_2 - 24 L \theta_3) = \frac{5 P}{16} \\
M_1 &= \frac{E I_z}{L^3}(-24 L v_2 + 4 L^2 \theta_2) = \frac{3 P L}{16}
\end{aligned}
$$

### Step 7: Check Global Equilibrium

**Sum of vertical forces:**

$$
F_y = \frac{11 P}{16} - P + \frac{5 P}{16} = 0
$$

**Sum of moments about node 1:**

$$
M = \frac{3 P L}{16} - \frac{P L}{2} + \frac{5 P}{16} L = 0
$$

Thus, the **finite element solution satisfies global equilibrium**.

---

## 📚 Reference

- [*Fundamentals of Finite Element Analysis - (Unit 2 - Chapter 4)*](Resources/FEM_Hutton_Unit2_Ch4.pdf) – Hutton David, McGraw-Hill 







