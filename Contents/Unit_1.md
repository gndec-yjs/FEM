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




