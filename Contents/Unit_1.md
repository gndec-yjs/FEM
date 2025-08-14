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

## Unit 1: Introduction

- [History and Applications of Finite Element Method](#history-and-applications-of-finite-element-method)  
- [Spring and Bar Elements](#spring-and-bar-elements)  
- [Minimum Potential Energy Principle](#minimum-potential-energy-principle)  
- [Direct Stiffness Method](#direct-stiffness-method)  
- [Nodal Equilibrium Equations](#nodal-equilibrium-equations)  
- [Assembly of Global Stiffness Matrix](#assembly-of-global-stiffness-matrix)  
- [Element Strain and Stress](#element-strain-and-stress)


## History and Applications of Finite Element Method

The **Finite Element Method (FEM)** is a powerful numerical technique used to obtain approximate solutions to a wide range of engineering problems, particularly those governed by differential equations. It is based on discretizing a structure or continuum into smaller parts, called *finite elements*, and formulating algebraic equations that can be solved using computational methods.


### 1. What is Finite Element Method?

In FEM, a physical domain is divided into a collection of smaller and simpler parts, known as **elements**, which are connected at points called **nodes**. The behaviour of each element is approximated using polynomial interpolation functions, and the system of equations is assembled for the entire structure. These equations are then solved to obtain an approximate solution.

The Finite Element Method is particularly useful when dealing with:

* Complex geometries
* Complicated boundary conditions
* Heterogeneous materials
* Irregular load distributions


### 2. Historical Background

The origin of FEM lies in both **mathematics** and **engineering mechanics**.

#### Key Milestones:

* **1943** – *Richard Courant*, a mathematician, introduced the concept of using piecewise polynomial interpolation over triangular subregions to solve the torsion problem. This laid the theoretical foundation of FEM.

* **1956** – *Turner, Clough, Martin, and Topp* applied the stiffness matrix approach to analyze aircraft structures. This marked the beginning of modern FEM in structural mechanics.

* **1960s–70s** – FEM was generalized to thermal, fluid, and electromagnetic problems and popularized with the emergence of computers.

* **1980s onwards** – Commercial FEA software such as ANSYS, ABAQUS, and NASTRAN emerged.

* **Today** – FEM is indispensable in multiple fields of engineering and science.

### 3. Applications of FEM in Engineering

The versatility of FEM lies in its ability to model and analyze various physical phenomena:

#### (a) Civil Engineering

* Structural analysis of buildings, bridges, dams
* Pavement design
* Earth-retaining structures
* Soil-structure interaction problems

#### (b) Mechanical Engineering

* Stress and strain analysis of machine components
* Heat conduction and convection in solids
* Vibration and dynamic analysis

#### (c) Aerospace Engineering

* Wing and fuselage structural analysis
* Jet engine thermal and stress modeling
* Fatigue and fracture analysis

#### (d) Biomechanics

* Stress analysis of bones and implants
* Tissue deformation modeling

#### (e) Electrical Engineering

* Electric field analysis
* Electromagnetic wave propagation

#### (f) Other Areas

* Automotive crash analysis
* Product design and optimization
* MEMS and nanotechnology

### 4. Advantages of FEM

* Can handle complex geometries and boundaries
* Suitable for both static and dynamic problems
* Easily programmable for use in software
* Applicable to linear and nonlinear analysis
* Modular and scalable in terms of elements

### 5. Limitations of FEM

* Requires high computational resources for large problems
* Accuracy depends on mesh quality and element type
* Needs understanding of boundary conditions and physical interpretation

### Summary:

The Finite Element Method is a cornerstone of modern engineering analysis, enabling engineers to simulate and predict the behavior of real-world systems under a variety of physical conditions. With a robust mathematical foundation and widespread software implementation, FEM is both an academic and industrial standard.

---

## **Spring and Bar Elements**

Spring and bar elements are the simplest structural elements in the finite element method.
They are used to model members that resist only axial deformation and serve as the foundation for understanding stiffness matrix formulation, nodal force–displacement relationships, and the assembly of element equations into a global system.

---

### **1. Spring Element**

A **spring element** is the simplest one-dimensional finite element. It is assumed to resist **only axial deformation** and is defined by:

* **Two nodes** — labelled $1$ and $2$
* **One displacement degree of freedom per node** — $u_1, u_2$
* **Linear elastic behavior** — force proportional to displacement difference

*(Insert Figure: Spring element with two nodes, displacements $u_1, u_2$, forces $f_1, f_2$)*

---

#### Force–Displacement Relationship

From **Hooke’s Law** for a linear spring:

$$
f = k(u_2 - u_1)
$$

where:

* $f$ = force in the spring (N)
* $k$ = spring stiffness (N/m)
* $u_1, u_2$ = nodal displacements (m)

At the individual nodes, the forces can be written as:

$$
f_1 = -k(u_2 - u_1)
$$

$$
f_2 = \phantom{-}k(u_2 - u_1)
$$

These satisfy **Newton’s third law** — equal and opposite forces.

---

#### Matrix Form of Element Equations

The nodal force–displacement relation can be written as:

$$
\begin{bmatrix}
f_1 \\
f_2
\end{bmatrix}
=
k
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
\begin{bmatrix}
u_1 \\
u_2
\end{bmatrix}
$$

The **element stiffness matrix** is therefore:

$$
[k] = k
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

---

#### Properties of the Spring Element

1. **Order of matrix:** $2 \times 2$ — because there are two DOFs.
2. **Symmetry:** $k_{12} = k_{21}$ — valid for linear elastic systems.
3. **Singular determinant:** Indicates rigid body motion if the element is unconstrained.
4. **Linearity:** Valid for small deformations with constant $k$.

---

**Figure Placeholders:**

* *(Insert Figure: Linear spring element showing nodes, displacements, and forces — fig\_spring\_element.png)*
* *(Insert Figure: Force–displacement graph of linear spring — fig\_spring\_graph.png)*

---

### **2. Bar Element in 1D**

A **bar element** is a straight structural member that resists only axial forces — either **tension** or **compression**.
It is one of the most basic finite elements used to model **trusses** and **axially loaded members** in frames.

---

#### Description and Assumptions

* **Two nodes** — labelled $1$ and $2$
* **One displacement degree of freedom per node** — $u_1, u_2$ (along the element axis)
* **Length** — $L$ (constant)
* **Cross-sectional area** — $A$ (uniform)
* **Material** — homogeneous and linearly elastic, modulus of elasticity $E$
* **Deformations** — small, so geometry changes are negligible
* **Loading** — purely axial (no bending or shear)

*(Insert Figure: Bar element showing length $L$, cross-section $A$, nodes 1 and 2, displacements $u_1, u_2$, and internal axial force $F$)*

---

#### Strain–Displacement Relation

The **axial strain** is defined as the change in length per unit original length:

$$
\varepsilon = \frac{u_2 - u_1}{L}
$$

Where $u_2 - u_1$ is the total elongation (or shortening) of the element.

---

#### Stress–Strain Law

From **Hooke’s Law** for axial loading:

$$
\sigma = E \cdot \varepsilon
$$

$$
\sigma = E \cdot \frac{u_2 - u_1}{L}
$$

Where:

* $\sigma$ = axial stress in the bar (N/m²)
* $E$ = Young’s modulus (Pa)

---

#### Force–Displacement Relation

The internal axial force in the bar is:

$$
F = \sigma \cdot A
$$

$$
F = \frac{EA}{L} (u_2 - u_1)
$$

This represents **tension** when $u_2 > u_1$ and **compression** when $u_2 < u_1$.

---

#### Nodal Force Equations

Considering equilibrium of each node:

$$
f_1 = -\frac{EA}{L} (u_2 - u_1)
$$

$$
f_2 = \phantom{-}\frac{EA}{L} (u_2 - u_1)
$$

Here $f_1$ and $f_2$ are the **nodal forces** (positive in tension).

---

#### Matrix Form of Element Equations

The above equations can be written compactly as:

$$
\begin{bmatrix}
f_1 \\
f_2
\end{bmatrix}
=
\frac{EA}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
\begin{bmatrix}
u_1 \\
u_2
\end{bmatrix}
$$

Thus, the **element stiffness matrix** is:

$$
[k] =
\frac{EA}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

---

#### Properties of the Bar Element

1. **Matrix size:** $2 \times 2$ (two DOFs)
2. **Symmetry:** stiffness matrix is symmetric ($k_{12} = k_{21}$)
3. **Positive semi-definite:** determinant zero if no boundary conditions are applied
4. **Linearity:** valid for constant $E$ and $A$ with small deformations

---

**Figure Placeholders:**

* *(Insert Figure: Bar element with nodes 1 & 2, axial displacements, and internal force — fig\_bar\_element.png)*
* *(Insert Figure: Free-body diagram of bar element — fig\_bar\_fbd.png)*

---
### **3. Assembly of Element Equations**

When several spring or bar elements are connected together to form a structure, their **element stiffness equations** must be combined into a **global stiffness matrix**. This process is called **assembly**.

The key idea:

* Each element stiffness matrix is defined in **local coordinates** between its own two nodes.
* The structure’s global stiffness matrix must relate **all nodal displacements** in the entire structure to the corresponding **global nodal forces**.

---

#### Procedure for Assembly

1. **Number the nodes** of the structure uniquely.
2. **Identify the degrees of freedom (DOFs)** for each node.
3. **Write the element stiffness matrix** for each element in terms of its end DOFs.
4. **Place the element stiffness coefficients** into the appropriate locations of the global stiffness matrix.
5. **Sum overlapping terms** where elements share a node.

---

#### Example: Two Bar Elements in Series

Consider two bar elements connected in series, forming three nodes:

* **Element 1:** connects Node 1 to Node 2, stiffness $k_1 = \frac{E_1A_1}{L_1}$
* **Element 2:** connects Node 2 to Node 3, stiffness $k_2 = \frac{E_2A_2}{L_2}$

From the individual element equations:

Element 1:

$$
\begin{bmatrix}
f_1 \\
f_2
\end{bmatrix}
=
k_1
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
\begin{bmatrix}
u_1 \\
u_2
\end{bmatrix}
$$

Element 2:

$$
\begin{bmatrix}
f_2' \\
f_3
\end{bmatrix}
=
k_2
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
\begin{bmatrix}
u_2 \\
u_3
\end{bmatrix}
$$

---

#### Forming the Global Stiffness Matrix

The total force at Node 2 is the sum of contributions from both elements:

$$
F_2 = f_2 + f_2'
$$

Combining all three nodes, the **global stiffness equation** is:

$$
\begin{bmatrix}
F_1 \\
F_2 \\
F_3
\end{bmatrix}
=
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1 + k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
\begin{bmatrix}
u_1 \\
u_2 \\
u_3
\end{bmatrix}
$$

Here:

* **Global stiffness matrix**: $3 \times 3$ — one DOF per node
* **Symmetric** and **banded** structure

---

#### Key Observations

* Off-diagonal terms are **negative** because they represent interaction between two connected DOFs.
* The diagonal term for a node is the **sum of the stiffnesses** of all elements connected to that node.
* If a node is free and not connected to any element, the corresponding row and column would be all zeros.

---

**Figure Placeholders (from Hutton’s book):**

* *(Insert Figure from Hutton: Fig. 2.9 — Spring elements connected in series)*
* *(Insert Figure from Hutton: Fig. 2.10 — Assembly of element stiffness into global stiffness matrix)*

---

### **4. Solved Example – Spring/Bar Element**

**Example:** (Adapted from Hutton)
A steel bar is composed of **two segments** joined in series.

* **Segment 1:** $L_1 = 600 \ \text{mm}$, $A_1 = 250 \ \text{mm}^2$, $E_1 = 200 \ \text{GPa}$
* **Segment 2:** $L_2 = 400 \ \text{mm}$, $A_2 = 300 \ \text{mm}^2$, $E_2 = 70 \ \text{GPa}$

The bar is fixed at the left end (Node 1) and subjected to a **tensile load** of $F = 50 \ \text{kN}$ at the free end (Node 3).

**Required:**

1. Displacement at the junction (Node 2) and at the free end (Node 3)
2. Element forces
3. Stresses in each segment

*(Insert Figure from Hutton: Fig. 2.13 — Two-segment bar with axial load and fixed support)*

---

#### **Step 1 – Calculate Element Stiffnesses**

For **Element 1** (Node 1–2):

$$
k_1 = \frac{E_1 A_1}{L_1}
$$

Convert units:
$E_1 = 200 \times 10^3 \ \text{MPa}$, $A_1 = 250 \ \text{mm}^2$, $L_1 = 600 \ \text{mm}$

$$
k_1 = \frac{(200 \times 10^3)(250)}{600} = 83\,333.33 \ \text{N/mm}
$$

For **Element 2** (Node 2–3):

$$
k_2 = \frac{E_2 A_2}{L_2}
$$

$$
k_2 = \frac{(70 \times 10^3)(300)}{400} = 52\,500 \ \text{N/mm}
$$

---

#### **Step 2 – Global Stiffness Matrix**

From assembly (Part 3):

$$
[K] =
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1 + k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
$$

Substitute $k_1, k_2$:

$$
[K] =
\begin{bmatrix}
83333.33 & -83333.33 & 0 \\
-83333.33 & 83333.33 + 52500 & -52500 \\
0 & -52500 & 52500
\end{bmatrix}
$$

$$
[K] =
\begin{bmatrix}
83333.33 & -83333.33 & 0 \\
-83333.33 & 135833.33 & -52500 \\
0 & -52500 & 52500
\end{bmatrix}
$$

---

#### **Step 3 – Apply Boundary Conditions**

Given:

* $u_1 = 0$ (fixed support at Node 1)
* Load: $F_3 = 50 \ \text{kN} = 50000 \ \text{N}$
* $F_2 = 0$ (no external load at Node 2)

The reduced system (removing first row/column for $u_1=0$):

$$
\begin{bmatrix}
135833.33 & -52500 \\
-52500 & 52500
\end{bmatrix}
\begin{bmatrix}
u_2 \\
u_3
\end{bmatrix}
=
\begin{bmatrix}
0 \\
50000
\end{bmatrix}
$$

---

#### **Step 4 – Solve for Displacements**

From the second equation:

$$
-52500 \cdot u_2 + 52500 \cdot u_3 = 50000
$$

$$
u_3 - u_2 = \frac{50000}{52500} \approx 0.95238 \ \text{mm}
$$

From the first equation:

$$
135833.33 \cdot u_2 - 52500 \cdot u_3 = 0
$$

Substitute $u_3 = u_2 + 0.95238$:

$$
135833.33 \cdot u_2 - 52500(u_2 + 0.95238) = 0
$$

$$
(135833.33 - 52500) u_2 = 52500 \cdot 0.95238
$$

$$
83333.33 \cdot u_2 = 50000
$$

$$
u_2 = 0.6 \ \text{mm}
$$

$$
u_3 = 0.6 + 0.95238 = 1.55238 \ \text{mm}
$$

---

#### **Step 5 – Element Forces and Stresses**

For Element 1:

$$
F_1 = k_1 (u_2 - u_1) = 83333.33 (0.6 - 0) = 50000 \ \text{N}
$$

$$
\sigma_1 = \frac{F_1}{A_1} = \frac{50000}{250} = 200 \ \text{MPa}
$$

For Element 2:

$$
F_2 = k_2 (u_3 - u_2) = 52500 (1.55238 - 0.6) \approx 50000 \ \text{N}
$$

$$
\sigma_2 = \frac{F_2}{A_2} = \frac{50000}{300} \approx 166.67 \ \text{MPa}
$$

---

#### **Final Answer:**

* $u_2 = 0.6 \ \text{mm}$
* $u_3 = 1.55238 \ \text{mm}$
* $F_1 = F_2 = 50 \ \text{kN}$
* $\sigma_1 = 200 \ \text{MPa}$
* $\sigma_2 \approx 166.67 \ \text{MPa}$

---

**Figure Placeholders (from Hutton’s book):**

* *(Insert Figure from Hutton: Fig. 2.13 — Two-segment bar with fixed support and axial load)*
* *(Insert Figure from Hutton: Fig. 2.14 — Displacement diagram for two-segment bar)*

---

## **5. Summary Table & Key Points — Spring and Bar Elements**

---

### **A. Summary Table**

| Feature / Formula     | **Spring Element**                               | **Bar Element (1D)**                                          |
| --------------------- | ------------------------------------------------ | ------------------------------------------------------------- |
| **DOFs per node**     | 1 (axial displacement)                           | 1 (axial displacement)                                        |
| **Material law**      | Hooke’s law: $f = k (u_2 - u_1)$                 | Hooke’s law: $\sigma = E \varepsilon$                         |
| **Strain relation**   | Not defined (spring elongation only)             | $\varepsilon = \frac{u_2 - u_1}{L}$                           |
| **Stress relation**   | Not applicable                                   | $\sigma = E \frac{u_2 - u_1}{L}$                              |
| **Element stiffness** | $k$ (given or computed)                          | $\frac{EA}{L}$                                                |
| **Stiffness matrix**  | $\begin{bmatrix} k & -k \\ -k & k \end{bmatrix}$ | $\frac{EA}{L} \begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}$ |
| **Typical units**     | N/m                                              | N/m                                                           |
| **Application areas** | Springs, supports, connectors                    | Truss members, axially loaded bars                            |
| **Assumptions**       | Linear elastic, small deformation                | Linear elastic, uniform cross-section, small deformation      |
| **Main output**       | Nodal forces and displacements                   | Nodal displacements, element forces, and stresses             |

---

### **B. Key Points to Remember**

1. **Foundation Elements**

   * Spring and bar elements form the basis for more complex elements (truss, beam, frame).

2. **Symmetric Stiffness Matrix**

   * Both have a $2 \times 2$ symmetric stiffness matrix.

3. **Force Equilibrium**

   * The sum of forces at any internal node is zero when no external load is applied.

4. **Assembly Rule**

   * Place each element stiffness matrix into the global stiffness matrix according to the global node numbering.

5. **Physical Meaning of $k$**

   * For springs: $k$ is given directly or determined experimentally.
   * For bars: $k = \frac{EA}{L}$ is derived from material and geometry.

6. **Units Consistency**

   * Always ensure $E$, $A$, and $L$ are in consistent units to avoid numerical errors.

7. **Load–Displacement Relationship**

   * Linear for these elements — doubling the load doubles the displacement.

---

## **Minimum Potential Energy Principle**

---

### **1. Introduction**

The **Principle of Minimum Potential Energy** is a fundamental concept in mechanics and the basis for many finite element formulations.
It states that:

> *Of all the possible displacement configurations that a structure can assume under given loads and boundary conditions, the one that actually occurs is the one for which the **total potential energy** is minimum.*

This principle provides an **energy-based route** to deriving equilibrium equations, rather than directly using Newton’s laws or force equilibrium.

*(Insert Figure from Hutton: Fig. 2.17 — Bar element showing displacement and forces for energy approach)*

---

### **2. Understanding Potential Energy in Structures**

The **total potential energy** of a deformable body is the sum of:

1. **Strain Energy (U)** – stored in the body due to deformation.
2. **Potential Energy of External Loads (V)** – work done by external forces.

$$
\Pi = U + V
$$

Where:

* $\Pi$ = total potential energy of the system
* $U$ = strain energy stored in elements
* $V$ = potential energy of applied forces (negative if loads do positive work)

---

#### **Strain Energy for a 1D Bar Element**

For a bar of length $L$, cross-section $A$, modulus $E$:

$$
U = \frac{1}{2} \int_{0}^{L} \sigma \varepsilon \, dV
$$

Using:

$$
\sigma = E \varepsilon, \quad \varepsilon = \frac{du}{dx}
$$

and $dV = A \, dx$:

$$
U = \frac{1}{2} \int_{0}^{L} E A \left( \frac{du}{dx} \right)^2 dx
$$

For a **linear displacement field** in a bar element:

$$
u(x) = N_1(x)u_1 + N_2(x)u_2
$$

where $N_1, N_2$ are shape functions.

---

#### **Potential Energy of External Nodal Loads**

If nodal loads $F_1, F_2$ are applied:

$$
V = - \left( F_1 u_1 + F_2 u_2 \right)
$$

(negative sign: loads doing positive work reduce total potential energy)

---

### **3. The Minimum Energy Condition**

The governing equation of the **minimum potential energy principle** is:

$$
\frac{\partial \Pi}{\partial u_i} = 0 \quad \text{for each DOF } u_i
$$

This condition ensures that total potential energy is stationary (minimum for stable equilibrium).

---

#### **For a 2-Node Bar Element:**

1. **Strain energy:**

$$
U = \frac{1}{2} \frac{EA}{L} (u_2 - u_1)^2
$$

2. **Potential of external loads:**

$$
V = -F_1 u_1 - F_2 u_2
$$

3. **Total potential energy:**

$$
\Pi = \frac{1}{2} \frac{EA}{L} (u_2 - u_1)^2 - F_1 u_1 - F_2 u_2
$$

4. **Apply minimum energy condition:**

   * Differentiate $\Pi$ with respect to $u_1$ and $u_2$ and set to zero:

$$
\frac{\partial \Pi}{\partial u_1} = \frac{EA}{L}(u_1 - u_2) - F_1 = 0
$$

$$
\frac{\partial \Pi}{\partial u_2} = \frac{EA}{L}(u_2 - u_1) - F_2 = 0
$$

5. **Matrix form:**

$$
\begin{bmatrix}
\frac{EA}{L} & -\frac{EA}{L} \\
-\frac{EA}{L} & \frac{EA}{L}
\end{bmatrix}
\begin{bmatrix}
u_1 \\ u_2
\end{bmatrix}
=
\begin{bmatrix}
F_1 \\ F_2
\end{bmatrix}
$$

This is exactly the **stiffness matrix equation** for the bar element derived earlier — showing how energy methods naturally lead to FEM formulations.

---

### **4. Advantages of Energy Approach in FEM**

* Avoids direct equilibrium force summation.
* Naturally incorporates material and geometric properties.
* Easily extended to more complex problems (beams, plates, shells).
* Forms the theoretical foundation for **Rayleigh–Ritz** and **Galerkin methods**.

---

### **5. Figure Placeholders (from Hutton’s book)**

* *(Insert Figure from Hutton: Fig. 2.17 — Bar element for potential energy derivation)*
* *(Insert Figure from Hutton: Fig. 2.18 — Work done by external nodal forces)*
* *(Insert Figure from Hutton: Fig. 2.19 — Flowchart of minimum potential energy application in FEM)*

---

## **Worked Example: Minimum Potential Energy Principle**

---

**Problem:**
A uniform steel bar of length $L = 2.0 \ \mathrm{m}$, cross-sectional area $A = 1.0 \times 10^{-4} \ \mathrm{m}^2$, and modulus of elasticity $E = 200 \ \mathrm{GPa}$ is fixed at the left end (Node 1) and subjected to an axial load $P = 10 \ \mathrm{kN}$ at the free end (Node 2).
Using the **principle of minimum potential energy**, determine the displacement at Node 2.

*(Insert Figure from Hutton: Fig. 2.17 — Bar element showing displacement and forces for energy approach)*

---

### **Step 1 — Total Potential Energy Expression**

We know:

$$
U = \frac{1}{2} \frac{EA}{L} (u_2 - u_1)^2
$$

Here $u_1 = 0$ (fixed), so:

$$
U = \frac{1}{2} \frac{EA}{L} (u_2)^2
$$

Potential of external loads:

$$
V = -P u_2
$$

Total potential energy:

$$
\Pi = \frac{1}{2} \frac{EA}{L} u_2^2 - P u_2
$$

---

### **Step 2 — Apply Minimum Energy Condition**

From the principle:

$$
\frac{\partial \Pi}{\partial u_2} = 0
$$

Differentiate:

$$
\frac{EA}{L} u_2 - P = 0
$$

---

### **Step 3 — Solve for $u_2$**

Substitute values:

$$
E = 2.0 \times 10^{11} \ \mathrm{Pa}
$$

$$
A = 1.0 \times 10^{-4} \ \mathrm{m}^2
$$

$$
L = 2.0 \ \mathrm{m}
$$

$$
P = 10,000 \ \mathrm{N}
$$

$$
\frac{(2.0 \times 10^{11})(1.0 \times 10^{-4})}{2.0} u_2 - 10,000 = 0
$$

$$
(1.0 \times 10^{7}) u_2 = 10,000
$$

$$
u_2 = 0.001 \ \mathrm{m} = 1.0 \ \mathrm{mm}
$$

---

### **Step 4 — Final Answer**

$$
\boxed{u_2 = 1.0 \ \mathrm{mm}}
$$

The free end of the bar displaces **1 mm** under the given load.

---

### **Step 5 — Key Observations from This Example**

* The energy method gives **exactly the same result** as the direct stiffness method for a simple bar.
* No direct equilibrium force summation was needed — the governing equation came naturally from the energy minimization process.
* This approach becomes more powerful for **multi-DOF systems** and **complex structures**.

---

**Figure Placeholders (from Hutton’s book):**

* *(Insert Figure from Hutton: Fig. 2.17 — Bar element for potential energy derivation)*
* *(Insert Figure from Hutton: Fig. 2.18 — Work done by external nodal forces)*

---

## **Generalization of the Minimum Potential Energy Principle to Multiple Degrees of Freedom**

---

### **1. Concept**

So far, we’ve applied the **Minimum Potential Energy Principle** to a single element with a single unknown displacement.
However, most FEM problems involve **many elements** and **multiple degrees of freedom (DOFs)**.

The principle generalizes naturally:

> *For a structure with $n$ degrees of freedom, the actual displacement configuration makes the **total potential energy** stationary (minimum) with respect to **each DOF**.*

Mathematically:

$$
\frac{\partial \Pi}{\partial u_i} = 0 \quad \text{for } i = 1, 2, \dots, n
$$

This leads directly to **$n$ simultaneous equilibrium equations**.

---

### **2. Multi-Element System: Strain Energy**

Consider a structure made of $m$ bar elements, each with stiffness matrix:

$$
[k]^{(e)} = \frac{E^{(e)} A^{(e)}}{L^{(e)}}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

The **strain energy** for element $e$ is:

$$
U^{(e)} = \frac{1}{2}
\begin{bmatrix}
u_i & u_j
\end{bmatrix}
[k]^{(e)}
\begin{bmatrix}
u_i \\ u_j
\end{bmatrix}
$$

where $u_i$ and $u_j$ are **global DOFs** corresponding to element nodes.

The **total strain energy** of the system is:

$$
U = \sum_{e=1}^{m} U^{(e)}
$$

---

### **3. Potential Energy of External Loads**

For nodal forces $F_1, F_2, \dots, F_n$:

$$
V = - \sum_{i=1}^{n} F_i u_i
$$

---

### **4. Total Potential Energy for the System**

$$
\Pi = U + V
$$

$$
\Pi = \frac{1}{2} \sum_{e=1}^{m}
\begin{bmatrix}
u_i & u_j
\end{bmatrix}
[k]^{(e)}
\begin{bmatrix}
u_i \\ u_j
\end{bmatrix}
- \sum_{i=1}^{n} F_i u_i
$$

---

### **5. Apply Minimum Energy Condition**

For each DOF $u_k$:

$$
\frac{\partial \Pi}{\partial u_k} = 0
$$

This yields **n equations**:

$$
[K] \{u\} = \{F\}
$$

where:

* $[K]$ = **global stiffness matrix** (assembled from element stiffness matrices)
* $\{u\}$ = vector of nodal displacements
* $\{F\}$ = vector of nodal forces

---

### **6. Connection to Direct Stiffness Method**

The process above is essentially the **energy-based derivation** of the **Direct Stiffness Method**:

1. Write strain energy for each element.
2. Sum to get total strain energy.
3. Subtract external work to get total potential energy.
4. Minimize with respect to each displacement DOF.
5. Obtain the same global stiffness equations as in the force equilibrium approach.

---

### **7. Example Structure for Illustration**

Consider a two-bar system:

* Bar 1 connects nodes 1–2
* Bar 2 connects nodes 2–3

Each bar obeys:

$$
[k]^{(e)} = \frac{EA}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

Assembly (from energy principle) gives:

$$
[K] =
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1 + k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
$$

*(Insert Figure from Hutton: Fig. 2.20 — Two-bar system for multi-DOF energy derivation)*

---

### **8. Advantages of the Multi-DOF Energy Approach**

* Same stiffness equations as direct stiffness method.
* Easier for **complex boundary conditions** and **variable material properties**.
* Naturally extends to **beams, frames, shells** using appropriate strain energy expressions.

---

**Figure Placeholders (from Hutton’s book):**

* *(Insert Figure from Hutton: Fig. 2.20 — Multi-element bar system)*
* *(Insert Figure from Hutton: Fig. 2.21 — Assembly of strain energy contributions)*
* *(Insert Figure from Hutton: Fig. 2.22 — Global stiffness matrix from energy principle)*

---

Here’s **Part 4 of Topic 3 — Summary Table + Key Points**, matching the style we used to wrap up Topic 2.

---

## **Summary Table + Key Points for Minimum Potential Energy Principle**

### **Summary Table**

| **Concept**                     | **Expression / Formula**                                           | **Remarks**                                                  |
| ------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------ |
| **Total Potential Energy**      | $\Pi = U + V$                                                      | Sum of strain energy $U$ and potential of external loads $V$ |
| **Strain Energy (1D Bar)**      | $U = \frac{1}{2} \frac{EA}{L} (u_2 - u_1)^2$                       | Based on Hooke’s law and linear displacement field           |
| **External Load Potential**     | $V = -\sum_{i=1}^{n} F_i u_i$                                      | Negative if loads do positive work                           |
| **Minimum Energy Condition**    | $\frac{\partial \Pi}{\partial u_i} = 0$                            | Applied for each degree of freedom                           |
| **Resulting Equations**         | $[K]\{u\} = \{F\}$                                                 | Leads to the same stiffness equations as force equilibrium   |
| **Element Stiffness (1D Bar)**  | $[k] = \frac{EA}{L} \begin{bmatrix}1 & -1 \\ -1 & 1\end{bmatrix}$  | Derived from energy minimization or direct stiffness method  |
| **Multi-Element Strain Energy** | $U = \frac{1}{2} \sum_{e=1}^m \{u^{(e)}\}^T [k]^{(e)} \{u^{(e)}\}$ | Summation over all elements in the system                    |

---

### **Key Points**

* **Energy-based formulation** provides an alternative to force equilibrium for deriving FEM equations.
* Works for **single-element** and **multi-element** systems with any number of DOFs.
* Requires **strain energy expressions** for elements and **external work terms**.
* Minimization of total potential energy yields the **global stiffness matrix** naturally.
* Especially useful in:

  * Problems with **complex geometry**
  * **Non-uniform material properties**
  * **Generalized coordinates** in structural mechanics
* The principle forms the theoretical basis of **Rayleigh–Ritz** and **Galerkin methods** used in advanced FEM.

---

## **Direct Stiffness Method**

---

### **1. Introduction**

The **Direct Stiffness Method** is the most widely used computational technique for solving structural mechanics problems in FEM.
It is a **systematic matrix-based procedure** that directly relates **nodal forces** to **nodal displacements** through the **stiffness matrix**.

It is the method implemented in almost all commercial finite element software.

---

### **2. Basic Concept**

For an element:

$$
\{f^{(e)}\} = [k]^{(e)} \{u^{(e)}\}
$$

Where:

* $\{f^{(e)}\}$ = element nodal force vector
* $[k]^{(e)}$ = element stiffness matrix in **global coordinates**
* $\{u^{(e)}\}$ = element nodal displacement vector in **global coordinates**

The global system is assembled by combining all element equations according to their node connectivity.

---

### **3. Steps in the Direct Stiffness Method**

---

#### **Step 1 — Discretize the Structure**

* Divide the structure into finite elements.
* Assign node numbers and element numbers.
* *(Insert Figure from Hutton: Fig. 2.23 — Example structure with node and element numbering)*

---

#### **Step 2 — Define Element Properties**

* Material properties: $E$, $A$, $I$, etc.
* Element geometry: length $L$, orientation $\theta$ (if 2D/3D).
* Cross-section details.

---

#### **Step 3 — Determine Element Stiffness Matrices**

For a **1D bar element** in global coordinates:

$$
[k]^{(e)} = \frac{EA}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

For a **2D/3D element**, transformation matrices are applied.

---

#### **Step 4 — Assemble the Global Stiffness Matrix**

Place each $[k]^{(e)}$ into the correct position of the global stiffness matrix $[K]$ according to **node connectivity**.

Example (two elements in series):

$$
[K] =
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1 + k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
$$

*(Insert Figure from Hutton: Fig. 2.24 — Assembly of global stiffness matrix)*

---

#### **Step 5 — Apply Boundary Conditions**

* For fixed supports: set corresponding displacement DOFs to zero and modify the system.
* For rollers or partial restraints: constrain only certain DOFs.

---

#### **Step 6 — Solve for Nodal Displacements**

The governing equation is:

$$
[K]\{u\} = \{F\}
$$

Where:

* $[K]$ = global stiffness matrix
* $\{u\}$ = unknown nodal displacements
* $\{F\}$ = known global nodal forces

---

#### **Step 7 — Compute Element Forces and Reactions**

After finding $\{u\}$:

1. **Element forces** in global coordinates:

$$
\{f^{(e)}\} = [k]^{(e)} \{u^{(e)}\}
$$

2. **Support reactions** from:

$$
\{R\} = [K] \{u\} - \{F\}
$$

---

### **4. Advantages of the Direct Stiffness Method**

* **Systematic** — easily programmable.
* Handles **complex geometries** and multiple load cases.
* Works directly in **matrix form** — ideal for computer implementation.
* Compatible with both **linear** and **nonlinear** analysis.

---

### **5. Comparison to Minimum Potential Energy Method**

| Direct Stiffness Method                     | Minimum Potential Energy Method      |
| ------------------------------------------- | ------------------------------------ |
| Based on **force equilibrium** at each node | Based on **energy minimization**     |
| Straightforward assembly of matrices        | Requires strain energy expressions   |
| Common in software packages                 | Used more in theoretical derivations |

---

**Figure Placeholders (from Hutton’s book):**

* *(Insert Figure: Fig. 2.23 — Node/element numbering example)*
* *(Insert Figure: Fig. 2.24 — Assembly of global stiffness matrix)*
* *(Insert Figure: Fig. 2.25 — Application of boundary conditions)*

---






