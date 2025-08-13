# MST-102: Finite Element Method in Structural Engineering  

## Unit 1: Introduction

- [History and Applications of Finite Element Method](#history-and-applications-of-finite-element-method)  
- [Spring and Bar Elements](#spring-and-bar-elements)  
- [Minimum Potential Energy Principle](#minimum-potential-energy-principle)  
- [Direct Stiffness Method](#direct-stiffness-method)  
- [Nodal Equilibrium Equations](#nodal-equilibrium-equations)  
- [Assembly of Global Stiffness Matrix](#assembly-of-global-stiffness-matrix)  
- [Element Strain and Stress](#element-strain-and-stress)


### History and Applications of Finite Element Method

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
### Spring and Bar Elements

Spring and bar elements are the simplest structural elements in finite element analysis.  
They form the foundation for understanding stiffness matrix formulation, nodal force–displacement relationships, and assembly procedures.

---

### 1. Spring Element

A spring element resists only axial deformation.  
It can be represented by two nodes, each with a single displacement degree of freedom.

**Hooke’s Law for a spring:**
f = k (u2 - u1)

Where:  
- f = force in the spring (N)  
- k = spring stiffness (N/m)  
- u1, u2 = nodal displacements (m)  

**Stiffness Matrix Derivation:**
The nodal force–displacement relationship in matrix form:

[ f1 ]   =  k * [  1   -1 ] [ u1 ]  
[ f2 ]              [ -1    1 ] [ u2 ]

The element stiffness matrix is:
[ k ] = k * [  1   -1  
              -1    1  ]

*(Insert Figure: Spring element with two nodes, displacements u1 & u2, and forces f1 & f2)*

---

### 2. Bar Element in 1D

A bar element is a straight structural member that resists only axial forces.

**Assumptions:**
- Material is linearly elastic  
- Cross-section is uniform  
- Deformation is small  
- Only axial displacement is considered  

**Strain–Displacement Relation:**
ε = (u2 - u1) / L  
Where L = length of bar element (m)  

**Stress–Strain Law:**
σ = E * ε = E * (u2 - u1) / L  
Where E = Young’s modulus (Pa)  

**Force–Displacement Relation:**
F = σ * A = (E * A / L) * (u2 - u1)  
Where A = cross-sectional area (m²)  

**Stiffness Matrix:**
[ f1 ]   =  (E*A/L) * [  1   -1 ] [ u1 ]  
[ f2 ]                     [ -1    1 ] [ u2 ]

Thus:
[ k ] = (E * A / L) * [  1   -1  
                         -1    1  ]

*(Insert Figure: Bar element with length L, area A, modulus E, nodes 1 & 2)*

---

### 3. Assembly of Element Equations

When multiple elements are connected, their stiffness matrices are combined into a **global stiffness matrix**.

**Example:** Two bar elements in series (nodes 1–2–3) with stiffnesses k1 and k2:

Global stiffness matrix:
[ K ] = [  k1     -k1       0  
          -k1   k1+k2    -k2  
           0     -k2      k2  ]

*(Insert Figure: Two bar elements in series with node connectivity diagram)*

---

### 4. Solved Example

**Problem:**  
A steel bar of length 2 m, cross-section 100 mm², and modulus E = 200 GPa is fixed at the left end and subjected to a 10 kN axial load at the right end.  
Find the displacement at the free end.

**Step 1: Convert units**  
A = 100 mm² = 1.0 × 10⁻⁴ m²  
E = 200 GPa = 2.0 × 10¹¹ Pa  
L = 2 m  

**Step 2: Calculate element stiffness**  
k = (E * A / L)  
  = (2.0 × 10¹¹ × 1.0 × 10⁻⁴) / 2  
  = 1.0 × 10⁷ N/m  

**Step 3: Apply boundary conditions**  
At node 1: u1 = 0 (fixed)  
At node 2: load F = 10,000 N  

**Step 4: Solve for displacement**  
F = k * (u2 - u1)  
10,000 = (1.0 × 10⁷) * (u2 - 0)  
u2 = 10,000 / (1.0 × 10⁷)  
u2 = 0.001 m = 1 mm  

**Answer:** Displacement at free end = **1 mm**

---

### Summary

- Spring and bar elements are the basic building blocks for FEM formulation.  
- They follow a linear force–displacement relationship from Hooke’s law.  
- Their stiffness matrices are simple and form the basis for complex element derivations in later topics.

---

\[
[K] =
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1 + k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
\]






