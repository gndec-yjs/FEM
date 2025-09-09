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

<img width="657" height="305" alt="image" src="https://github.com/user-attachments/assets/97ca0ee6-10c6-47db-92ad-533a819c0d2b" />

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

### **2. Bar Element in 1D**

A **bar element** is a straight structural member that resists only axial forces — either **tension** or **compression**.
It is one of the most basic finite elements used to model **trusses** and **axially loaded members** in frames.

<img width="297" height="173" alt="image" src="https://github.com/user-attachments/assets/bd322428-7c08-4b01-8a10-61ad8d9b6608" />

#### Description and Assumptions

* **Two nodes** — labelled $1$ and $2$
* **One displacement degree of freedom per node** — $u_1, u_2$ (along the element axis)
* **Length** — $L$ (constant)
* **Cross-sectional area** — $A$ (uniform)
* **Material** — homogeneous and linearly elastic, modulus of elasticity $E$
* **Deformations** — small, so geometry changes are negligible
* **Loading** — purely axial (no bending or shear)

#### Formulation of the finite element characteristics of an elastic bar element is based on the following assumptions:

1. The bar is geometrically straight.
2. The material obeys Hooke’s law.
3. Forces are applied only at the ends of the bar.
4. The bar supports axial loading only; bending, torsion, and shear are not transmitted to the element via the nature of its connections to other  elements.

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
### Example: Two Spring Elements in Series

Consider two spring elements connected in series, forming three nodes:

<img width="464" height="157" alt="image" src="https://github.com/user-attachments/assets/547ab6ec-bea6-4b32-ba1d-f7c7a7d797c2" />

- **Element 1**: connects Node 1 to Node 2, stiffness $k_1 = \frac{E_{1} A_{1}}{L_{1}}$  
- **Element 2**: connects Node 2 to Node 3, stiffness $k_2 = \frac{E_{2} A_{2}}{L_{2}}$

<img width="455" height="212" alt="image" src="https://github.com/user-attachments/assets/6f0263f7-4fd3-4be9-8129-aad56b7f28a9" />


#### Element Equations

For **Element 1**:

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

For **Element 2**:

$$
\begin{bmatrix}
f'_2 \\
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

#### Combining Forces at Node 2

The total force at Node 2 is the sum of contributions from both elements:

$$
F_2 = f_2 + f'_2
$$

#### Global Stiffness Equation

Collecting contributions for all three nodes, we obtain:

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


#### Observations:
- The **global stiffness matrix** is \( 3 \times 3 \), since there is **one degree of freedom per node**.  
- It is **symmetric** and has a **banded structure**, typical of FEM formulations.  

---

#### Example: Two Bar Elements in Series

Consider two bar elements connected in series, forming three nodes:

<img width="416" height="157" alt="image" src="https://github.com/user-attachments/assets/2f698902-16b0-43b4-a1e8-eeddeae5c8ae" />

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

### **4. Solved Example – Spring/Bar Element**

### Example 1: Two-Element Spring System 

**Problem:**  
Consider the two-element system shown in Figure below.

<img width="464" height="157" alt="image" src="https://github.com/user-attachments/assets/547ab6ec-bea6-4b32-ba1d-f7c7a7d797c2" />

- Node 1 fixed: $$U_1 = 0$$  
- Spring stiffnesses: $$k_1 = 50 \ \text{lb/in}, \ k_2 = 75 \ \text{lb/in}$$  
- External forces: $$F_2 = 75 \ \text{lb}, \ F_3 = 75 \ \text{lb}$$  

Determine the nodal displacements $$U_2$$ and $$U_3$$.

---

#### Step 1: Global Stiffness Matrix

The global finite element equations are

$$
\begin{bmatrix}
F_1 \\[6pt]
F_2 \\[6pt]
F_3
\end{bmatrix}
=
\begin{bmatrix}
k_1 & -k_1 & 0 \\[6pt]
-k_1 & k_1 + k_2 & -k_2 \\[6pt]
0 & -k_2 & k_2
\end{bmatrix}
\begin{bmatrix}
U_1 \\[6pt]
U_2 \\[6pt]
U_3
\end{bmatrix}
$$

Substitute values $$k_1 = 50, \ k_2 = 75$$:

$$
\begin{bmatrix}
F_1 \\[6pt]
F_2 \\[6pt]
F_3
\end{bmatrix}
=
\begin{bmatrix}
50 & -50 & 0 \\[6pt]
-50 & 125 & -75 \\[6pt]
0 & -75 & 75
\end{bmatrix}
\begin{bmatrix}
U_1 \\[6pt]
U_2 \\[6pt]
U_3
\end{bmatrix}
$$

---

#### Step 2: Apply Boundary Condition $$U_1 = 0$$

Constraint equation from Row 1:

$$
-50 U_2 = F_1
$$

Active system (Rows 2 and 3):

$$
\begin{bmatrix}
125 & -75 \\[6pt]
-75 & 75
\end{bmatrix}
\begin{bmatrix}
U_2 \\[6pt]
U_3
\end{bmatrix}
=
\begin{bmatrix}
75 \\[6pt]
75
\end{bmatrix}
$$

---

#### Step 3: Solve for Displacements

Equation (2):  

$$
125 U_2 - 75 U_3 = 75
$$

Equation (3):  

$$
-75 U_2 + 75 U_3 = 75
$$

From Equation (3):  

$$
75 (U_3 - U_2) = 75
\;\;\Rightarrow\;\;
U_3 = U_2 + 1
$$

Substitute into Equation (2):  

$$
125 U_2 - 75 (U_2 + 1) = 75
$$

$$
125 U_2 - 75 U_2 - 75 = 75
$$

$$
50 U_2 = 150
\;\;\Rightarrow\;\;
U_2 = 3 \ \text{in}
$$

Back-substitute:

$$
U_3 = U_2 + 1 = 3 + 1 = 4 \ \text{in}
$$

---

#### Step 4: Reaction at Node 1

From constraint equation:

$$
F_1 = -50 U_2 = -50 (3) = -150 \ \text{lb}
$$

---

### Final Results

$$U_2 = 3 \ \text{in}$$  

$$U_3 = 4 \ \text{in}$$  

$$F_1 = -150 \ \text{lb}$$  

Check equilibrium:

$$
F_1 + F_2 + F_3 = (-150) + 75 + 75 = 0 \quad \checkmark
$$


---

**Example 2:** A steel bar is composed of **two segments** joined in series.

* **Segment 1:** $L_1 = 600 \ \text{mm}$, $A_1 = 250 \ \text{mm}^2$, $E_1 = 200 \ \text{GPa}$
* **Segment 2:** $L_2 = 400 \ \text{mm}$, $A_2 = 300 \ \text{mm}^2$, $E_2 = 70 \ \text{GPa}$

<img width="1162" height="301" alt="image" src="https://github.com/user-attachments/assets/f543f5d3-be9e-4c4a-a396-1914f3d50c56" />

The bar is fixed at the left end (Node 1) and subjected to a **tensile load** of $F = 50 \ \text{kN}$ at the free end (Node 3).

**Find:**

1. Displacement at the junction (Node 2) and at the free end (Node 3)
2. Element forces
3. Stresses in each segment


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

$u_2 = 0.6 \ \text{mm}$
$u_3 = 1.55238 \ \text{mm}$
$F_1 = F_2 = 50 \ \text{kN}$
$\sigma_1 = 200 \ \text{MPa}$
$\sigma_2 \approx 166.67 \ \text{MPa}$

---

### Example 3: Three Springs Supporting Three Equal Weights

**Problem statement:**  

Figure depicts a system of three linearly elastic springs supporting three equal weights W suspended in a vertical plane. Treating the springs as finite elements, determine the vertical displacement of each weight.

<img width="242" height="375" alt="image" src="https://github.com/user-attachments/assets/13bf0445-7b89-4604-80c8-caf2e5759508" />

---

#### Step 1: Element Stiffness Matrices

Each spring element stiffness matrix is (general form):

$$
k^{(e)} =
\begin{bmatrix}
k & -k \\
-k & k
\end{bmatrix}
$$

For the three springs:

- Element (1), stiffness = $$3k$$  
$$
k^{(1)} =
\begin{bmatrix}
3k & -3k \\
-3k & 3k
\end{bmatrix}
$$

- Element (2), stiffness = $$2k$$  
$$
k^{(2)} =
\begin{bmatrix}
2k & -2k \\
-2k & 2k
\end{bmatrix}
$$

- Element (3), stiffness = $$k$$  
$$
k^{(3)} =
\begin{bmatrix}
k & -k \\
-k & k
\end{bmatrix}
$$

---

#### Step 2: Element-to-Global DOF Relations

Assign global displacements: $$U_1, U_2, U_3, U_4$$.  

Mapping:

- $$u^{(1)}_1 = U_1, \quad u^{(1)}_2 = U_2$$  
- $$u^{(2)}_1 = U_2, \quad u^{(2)}_2 = U_3$$  
- $$u^{(3)}_1 = U_3, \quad u^{(3)}_2 = U_4$$  

---

#### Step 3: Assemble Global Stiffness Matrix

Expanding each element stiffness matrix into the global 4×4 form:

- For element (1):

$$
\begin{bmatrix}
3k & -3k & 0 & 0 \\
-3k & 3k & 0 & 0 \\
0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

- For element (2):

$$
\begin{bmatrix}
0 & 0 & 0 & 0 \\
0 & 2k & -2k & 0 \\
0 & -2k & 2k & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

- For element (3):

$$
\begin{bmatrix}
0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 \\
0 & 0 & k & -k \\
0 & 0 & -k & k
\end{bmatrix}
$$

Adding them gives the global stiffness matrix:

$$
K =
k \begin{bmatrix}
3 & -3 & 0 & 0 \\
-3 & 5 & -2 & 0 \\
0 & -2 & 3 & -1 \\
0 & 0 & -1 & 1
\end{bmatrix}
$$

---

#### Step 4: Global Equilibrium Equations

The global system is:

$$
K \, U = F
$$

That is:

$$
k
\begin{bmatrix}
3 & -3 & 0 & 0 \\
-3 & 5 & -2 & 0 \\
0 & -2 & 3 & -1 \\
0 & 0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
U_1 \\ U_2 \\ U_3 \\ U_4
\end{bmatrix}
=
\begin{bmatrix}
F_1 \\ W \\ W \\ W
\end{bmatrix}
$$

---

#### Step 5: Apply Constraint

Node 1 is fixed: $$U_1 = 0$$.  

Constraint equation:

$$
-3k U_2 = F_1
$$

Active system (removing row 1, col 1):

$$
k
\begin{bmatrix}
5 & -2 & 0 \\
-2 & 3 & -1 \\
0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
U_2 \\ U_3 \\ U_4
\end{bmatrix}
=
\begin{bmatrix}
W \\ W \\ W
\end{bmatrix}
$$

---

#### Step 6: Solve System

The equations are:

1) $$5 U_2 - 2 U_3 = \tfrac{W}{k}$$
2) $$-2 U_2 + 3 U_3 - U_4 = \tfrac{W}{k}$$  
3) $$-U_3 + U_4 = \tfrac{W}{k}$$

From (3):  

$$
U_4 = U_3 + \tfrac{W}{k}
$$

Substitute into (2):  

$$
-2 U_2 + 3 U_3 - (U_3 + \tfrac{W}{k}) = \tfrac{W}{k}
$$

$$
-2 U_2 + 2 U_3 = 2 \tfrac{W}{k}
\;\;\Rightarrow\;\;
- U_2 + U_3 = \tfrac{W}{k}
$$

So:  

$$
U_3 = U_2 + \tfrac{W}{k}
$$

Now substitute into (1):  

$$
5 U_2 - 2 (U_2 + \tfrac{W}{k}) = \tfrac{W}{k}
$$

$$
5 U_2 - 2 U_2 - 2 \tfrac{W}{k} = \tfrac{W}{k}
$$

$$
3 U_2 = 3 \tfrac{W}{k}
\;\;\Rightarrow\;\;
U_2 = \tfrac{W}{k}
$$

Then:  

$$
U_3 = U_2 + \tfrac{W}{k} = 2 \tfrac{W}{k}
$$

$$
U_4 = U_3 + \tfrac{W}{k} = 3 \tfrac{W}{k}
$$

---

#### Step 7: Reaction Force at Node 1

From constraint equation:  

$$
F_1 = -3k U_2 = -3k \left(\tfrac{W}{k}\right) = -3W
$$

---

### Final Results

$$U_1 = 0$$  
$$U_2 = \tfrac{W}{k}$$  
$$U_3 = \tfrac{2W}{k}$$  
$$U_4 = \tfrac{3W}{k}$$  
Reaction: $$F_1 = -3W$$  

---

**Interpretation:**  
The displacements increase linearly with the node number since each spring carries the same load. The reaction balances the three applied weights.


---
### Note the general procedure as follows:

-  Formulate the individual element stiffness matrices.
- Write the element to global displacement relations.
- Assemble the global equilibrium equation in matrix form.
- Reduce the matrix equations according to specified constraints.
- Solve the system of equations for the unknown nodal displacements (primary variables).
- Solve for the reaction forces (secondary variable) by back-substitution.

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

### **1. Introduction**

The **Principle of Minimum Potential Energy** is a fundamental concept in mechanics and the basis for many finite element formulations.
It states that:

> *Of all displacement states of a body or structure, subjected to external loading, that satisfy the geometric boundary conditions (imposed displacements), the displacement state that also satisfies the equilibrium equations is such that the total potential energy is a minimum for stable equilibrium.*

In simple words, 

> *Of all the possible displacement configurations that a structure can assume under given loads and boundary conditions, the one that actually occurs is the one for which the **total potential energy** is minimum.*

This principle provides an **energy-based route** to deriving equilibrium equations, rather than directly using Newton’s laws or force equilibrium.

---

### **2. Understanding Potential Energy in Structures**

The **total potential energy** of a deformable body is the sum of:

1. **Strain Energy (U or U<sub>e</sub>)** – stored in the body due to deformation.
2. **Potential Energy of External Loads (V or U<sub>f</sub>)** – work done by external forces.

$$
\Pi = U + V
$$

Where:

* $\Pi$ = total potential energy of the system
* $U$ or U<sub>e</sub> = strain energy stored in elements
* $V$ or U<sub>f</sub> = potential energy of applied forces (negative if loads do positive work)

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

where $N_1, N_2$ are 
<span class="hover-term" title=" In the context of the Finite Element Method (FEM), shape functions are mathematical tools used to interpolate unknown field variables—like displacement, temperature, or stress—within an element based on values at its nodes."> **_shape functions_**.</span>

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

1) **Strain energy:**

$$
U = \frac{1}{2} \frac{EA}{L} (u_2 - u_1)^2
$$

2) **Potential of external loads:**

$$
V = -F_1 u_1 - F_2 u_2
$$

3) **Total potential energy:**

$$
\Pi = \frac{1}{2} \frac{EA}{L} (u_2 - u_1)^2 - F_1 u_1 - F_2 u_2
$$

4) **Apply minimum energy condition:**

   * Differentiate $\Pi$ with respect to $u_1$ and $u_2$ and set to zero:

$$
\frac{\partial \Pi}{\partial u_1} = \frac{EA}{L}(u_1 - u_2) - F_1 = 0
$$

$$
\frac{\partial \Pi}{\partial u_2} = \frac{EA}{L}(u_2 - u_1) - F_2 = 0
$$

5) **Matrix form:**

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

## **Worked Example: Minimum Potential Energy Principle**

---

**Problem:**
A uniform steel bar of length $L = 2.0 \ \mathrm{m}$, cross-sectional area $A = 1.0 \times 10^{-4} \ \mathrm{m}^2$, and modulus of elasticity $E = 200 \ \mathrm{GPa}$ is fixed at the left end (Node 1) and subjected to an axial load $P = 10 \ \mathrm{kN}$ at the free end (Node 2).

<img width="1184" height="201" alt="image" src="https://github.com/user-attachments/assets/0aaa5018-b2a0-4f37-8d5d-1163b99b71c4" />

Using the **principle of minimum potential energy**, determine the displacement at Node 2.

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

## **Generalization of the Minimum Potential Energy Principle to Multiple Degrees of Freedom**

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

---

### **8. Advantages of the Multi-DOF Energy Approach**

* Same stiffness equations as direct stiffness method.
* Easier for **complex boundary conditions** and **variable material properties**.
* Naturally extends to **beams, frames, shells** using appropriate strain energy expressions.

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

<img width="470" height="163" alt="image" src="https://github.com/user-attachments/assets/eaeda008-9c61-4c26-bd9e-8d99de110afa" />

---

#### **Step 2 — Define Element Properties**

* Material properties: $E$, $A$, $I$, etc.
* Element geometry: length $L$, orientation $\theta$ (if 2D/3D).
* Cross-section details.

<img width="494" height="278" alt="image" src="https://github.com/user-attachments/assets/7dba01c6-ca43-40ed-9514-588ffcbb64cf" />

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

## **Detailed Worked Example (Direct Stiffness Method)**

**Problem**
A prismatic steel bar is modeled with **three axial (bar) elements** in series: Nodes 1-2-3-4.

* Modulus: $E = 200{,}000 \ \text{MPa} = 200{,}000 \ \text{N/mm}^2$
* Area: $A = 100 \ \text{mm}^2$ (constant)
* Lengths: $L_1 = 500 \ \text{mm}, \quad L_2 = 500 \ \text{mm}, \quad L_3 = 1000 \ \text{mm}$
* Boundary condition: **Node 1 fixed**, $u_1 = 0$
* Loads: $F_3 = 30 \ \text{kN}$ (tension), $F_4 = 20 \ \text{kN}$ (tension), $F_2 = 0$

*(Insert Figure from Hutton — **Direct Stiffness example**: node & element numbering, axial bar with loads; Chapter 2 near “Direct Stiffness Method”)*

### **1) Element Stiffness Matrices**

For a 1D bar element,

$$
[k]^{(e)}=\frac{EA}{L_e}
\begin{bmatrix}
1 & -1\\
-1 & 1
\end{bmatrix}
\quad\Rightarrow\quad
k_e=\frac{EA}{L_e}
$$

Compute $k_e$ (units N/mm):

$$
EA = (200{,}000)(100)=20{,}000{,}000 \ \text{N}
$$

$$
k_1=\frac{20{,}000{,}000}{500}=40{,}000,\quad
k_2=\frac{20{,}000{,}000}{500}=40{,}000,\quad
k_3=\frac{20{,}000{,}000}{1000}=20{,}000
$$

*(Insert Figure from Hutton — **Local 2-node bar stiffness** and sign convention)*

### **2) Assemble the Global Stiffness Matrix**

Connectivity (global DOFs are nodal displacements $u_1,u_2,u_3,u_4$):

$$
[K]=
\begin{bmatrix}
k_1 & -k_1 & 0 & 0\\
-k_1 & k_1{+}k_2 & -k_2 & 0\\
0 & -k_2 & k_2{+}k_3 & -k_3\\
0 & 0 & -k_3 & k_3
\end{bmatrix}
=
\begin{bmatrix}
40000 & -40000 & 0 & 0\\
-40000 & 80000 & -40000 & 0\\
0 & -40000 & 60000 & -20000\\
0 & 0 & -20000 & 20000
\end{bmatrix}
$$

*(Insert Figure from Hutton — **Assembly diagram** showing overlapping stiffness contributions into $[K]$)*

### **3) Apply Boundary Conditions and Loads**

* Essential BC: $u_1=0$
* Load vector: $\{F\}=[0,\;0,\;30000,\;20000]^T \ \text{N}$

Reduce the system by removing row/column for $u_1$:

$$
[K_r]=
\begin{bmatrix}
80000 & -40000 & 0\\
-40000 & 60000 & -20000\\
0 & -20000 & 20000
\end{bmatrix},\quad
\{u_r\}=
\begin{bmatrix}
u_2\\u_3\\u_4
\end{bmatrix},\quad
\{F_r\}=
\begin{bmatrix}
0\\30000\\20000
\end{bmatrix}
$$

*(Insert Figure from Hutton — **Boundary condition imposition** & reduced system depiction)*

### **4) Solve for Nodal Displacements**

$$
[K_r]\{u_r\}=\{F_r\}
\quad\Rightarrow\quad
\{u_r\}=
\begin{bmatrix}
1.25\\[2pt]
2.50\\[2pt]
3.50
\end{bmatrix}\ \text{mm}
$$

Hence:

$$
u_1=0,\quad u_2=1.25\ \text{mm},\quad u_3=2.50\ \text{mm},\quad u_4=3.50\ \text{mm}
$$

### **5) Element Forces and Stresses (Post-processing)**

Element axial forces (tension positive):

$$
F_1 = k_1(u_2-u_1)=40000(1.25-0)=50{,}000\ \text{N}=50\ \text{kN}
$$

$$
F_2 = k_2(u_3-u_2)=40000(2.50-1.25)=50\ \text{kN}
$$

$$
F_3 = k_3(u_4-u_3)=20000(3.50-2.50)=20\ \text{kN}
$$

Element stresses $\sigma_e=F_e/A$ (with $A=100\ \text{mm}^2$):

$$
\sigma_1=\frac{50{,}000}{100}=500\ \text{MPa},\quad
\sigma_2=500\ \text{MPa},\quad
\sigma_3=\frac{20{,}000}{100}=200\ \text{MPa}
$$

*(Insert Figure from Hutton — **Element force directions & sign convention**; **stress distribution** sketch)*

### **6) Support Reaction Check**

Compute reactions via $\{R\}=[K]\{u\}-\{F\}$ (using the full system):

$$
R_1 = 50 \ \text{kN},\quad R_2=R_3=R_4=0
$$

**Equilibrium check:** $R_1 = F_3+F_4 = 30+20=50\ \text{kN}$ ✓

*(Insert Figure from Hutton — **Support reaction extraction** illustration)*

### **7) What This Example Demonstrates**

1. Clean **DSM workflow**: element $k_e$ → assemble $[K]$ → apply BCs → solve $\{u\}$ → recover forces/stresses → reactions.
2. **Physical consistency**: internal element forces match applied nodal loads; reactions satisfy global equilibrium.
3. **Scalability**: the exact same steps extend to 2D/3D trusses, frames (with rotation DOFs), and beyond.

---

### **Figure Placeholders from Hutton (drop where indicated above)**

* *Insert Hutton figure: Node & element numbering for a multi-element bar (Ch. 2, Direct Stiffness section).*
* *Insert Hutton figure: Local 2-node bar stiffness & sign conventions (Ch. 2).*
* *Insert Hutton figure: Global assembly illustration for serial bar elements (Ch. 2).*
* *Insert Hutton figure: Imposing boundary conditions / reduced system (Ch. 2).*
* *Insert Hutton figure: Reaction recovery and equilibrium check (Ch. 2).*

---

## **Direct Stiffness Method — At a Glance**

### **Step-by-Step Checklist**

1. **Discretize the Structure**

   * Define nodes and elements
   * Assign numbers and connectivity
     *(Insert Figure Placeholder: simple bar/truss with nodes labeled)*

2. **Define Element Properties**

   * $E$, $A$, $I$, length $L$, and orientation (if 2D/3D)

3. **Form Local Element Stiffness Matrices**

   * For 1D bar:

     $$
     [k]^{(e)}=\frac{EA}{L_e}
     \begin{bmatrix}
     1 & -1\\
     -1 & 1
     \end{bmatrix}
     $$
   * Apply transformation if needed (2D/3D)

4. **Assemble Global Stiffness Matrix**

   * Place element matrices into the correct positions in $[K]$ using node connectivity

5. **Apply Boundary Conditions**

   * Modify $[K]$ and $\{F\}$ for prescribed displacements

6. **Solve for Nodal Displacements**

   * $[K]\{u\}=\{F\}$

7. **Post-process**

   * **Element forces**: $\{f^{(e)}\}=[k]^{(e)}\{u^{(e)}\}$
   * **Stresses**: $\sigma = F/A$
   * **Reactions**: $\{R\}=[K]\{u\}-\{F\}$

---

### **Direct Stiffness Method (DSM) Flowchart**

**Flow:**
Discretize → Define Properties → Form $[k]^{(e)}$ → Assemble $[K]$ → Apply BCs → Solve $\{u\}$ → Post-process

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 920 1200" role="img" aria-labelledby="title desc" style="max-width:100%; height:auto;">
  <title id="title">Direct Stiffness Method Flowchart</title>
  <desc id="desc">Flowchart showing DSM steps from discretization to checks.</desc>
  <style>
    .box{fill:#ffffff;stroke:#222;stroke-width:2;rx:8;ry:8}
    .arrow{stroke:#222;stroke-width:2;marker-end:url(#arrow)}
    .text{font-family:system-ui,Segoe UI,Helvetica,Arial,sans-serif;font-size:16px;fill:#111}
    .head{font-weight:700;font-size:18px}
    .code{font-family:ui-monospace,Consolas,Monaco,monospace}
  </style>
  <defs>
    <marker id="arrow" markerWidth="12" markerHeight="12" refX="10" refY="6" orient="auto">
      <path d="M0,0 L12,6 L0,12 z" fill="#222"/>
    </marker>
  </defs>

  <!-- Header -->
  <rect class="box" x="160" y="20" width="600" height="60"/>
  <text class="text head" x="460" y="58" text-anchor="middle">DIRECT STIFFNESS METHOD</text>

  <!-- 1 Discretize -->
  <rect class="box" x="140" y="110" width="640" height="90"/>
  <text class="text head" x="160" y="140">1) Discretize Structure</text>
  <text class="text" x="160" y="165">• Define nodes & elements; number nodes (global DOFs)</text>

  <!-- Arrow -->
  <line class="arrow" x1="460" y1="200" x2="460" y2="230"/>

  <!-- 2 Properties -->
  <rect class="box" x="140" y="230" width="640" height="90"/>
  <text class="text head" x="160" y="260">2) Define Properties</text>
  <text class="text" x="160" y="285">• E, A, I, geometry (L), orientation (2D/3D)</text>

  <line class="arrow" x1="460" y1="320" x2="460" y2="350"/>

  <!-- 3 Element stiffness (local) -->
  <rect class="box" x="140" y="350" width="640" height="110"/>
  <text class="text head" x="160" y="380">3) Element Stiffness [k] (local)</text>
  <text class="text code" x="160" y="405">Bars: [k] = (EA/L) [[1,-1],[-1,1]]</text>
  <text class="text" x="160" y="428">Beams/Frames: use appropriate local matrices</text>

  <!-- Split arrows to transform / skip -->
  <line class="arrow" x1="460" y1="460" x2="300" y2="490"/>
  <line class="arrow" x1="460" y1="460" x2="620" y2="490"/>

  <!-- 3a Transform (left) -->
  <rect class="box" x="80" y="490" width="360" height="90"/>
  <text class="text head" x="100" y="520">3a) Transform to Global (if 2D/3D)</text>
  <text class="text code" x="100" y="545">[k]_g = Tᵀ [k]_local T</text>

  <!-- 3a Skip (right) -->
  <rect class="box" x="480" y="490" width="360" height="90"/>
  <text class="text head" x="500" y="520">3a) Skip Transform (1D axial)</text>
  <text class="text code" x="500" y="545">[k]_g = [k]_local</text>

  <!-- Merge to Assemble -->
  <line class="arrow" x1="260" y1="580" x2="260" y2="610"/>
  <line class="arrow" x1="700" y1="580" x2="700" y2="610"/>
  <line class="arrow" x1="260" y1="610" x2="460" y2="640"/>
  <line class="arrow" x1="700" y1="610" x2="460" y2="640"/>

  <!-- 4 Assemble -->
  <rect class="box" x="140" y="640" width="640" height="100"/>
  <text class="text head" x="160" y="670">4) Assemble Global [K]</text>
  <text class="text" x="160" y="695">• Scatter/gather each [k]_g into [K] using connectivity</text>
  <text class="text" x="160" y="718">• Sum overlapping entries at shared DOFs</text>

  <line class="arrow" x1="460" y1="740" x2="460" y2="770"/>

  <!-- 5 BCs -->
  <rect class="box" x="140" y="770" width="640" height="90"/>
  <text class="text head" x="160" y="800">5) Apply Boundary Conditions</text>
  <text class="text" x="160" y="825">• Prescribed displacements → modify [K], {F}</text>

  <line class="arrow" x1="460" y1="860" x2="460" y2="890"/>

  <!-- 6 Solve -->
  <rect class="box" x="140" y="890" width="640" height="85"/>
  <text class="text head" x="160" y="920">6) Solve for {u}</text>
  <text class="text code" x="160" y="945">[K]{u} = {F}</text>

  <line class="arrow" x1="460" y1="975" x2="460" y2="1005"/>

  <!-- 7 Post-process + 8 Checks (merged) -->
  <rect class="box" x="140" y="1005" width="640" height="140"/>
  <text class="text head" x="160" y="1035">7) Post-process  &  8) Checks</text>
  <text class="text" x="160" y="1060">• Element forces: {f} = [k]_e {u}_e;  Stresses: σ = F/A</text>
  <text class="text" x="160" y="1083">• Reactions: {R} = [K]{u} − {F};  Equilibrium & BC verification </text>
</svg>

---

## **Practice Problems**

**Problem 1 — Single Bar Element**
A steel rod of length $L = 2 \ \mathrm{m}$, $E = 210 \ \mathrm{GPa}$, $A = 150 \ \mathrm{mm}^2$ is fixed at one end and subjected to a tensile load of $25 \ \mathrm{kN}$ at the free end.

* (a) Form the element stiffness matrix.
* (b) Find the displacement at the free end.
* (c) Find the axial stress in the bar.

---

**Problem 2 — Two Elements in Series**
Two steel bars ($E = 200 \ \mathrm{GPa}$, $A = 100 \ \mathrm{mm}^2$) are connected in series:

* $L_1 = 400 \ \mathrm{mm}$
* $L_2 = 600 \ \mathrm{mm}$
  Node 1 is fixed, a load of $20 \ \mathrm{kN}$ is applied at Node 3.
* (a) Write the global stiffness matrix.
* (b) Determine nodal displacements $u_2$ and $u_3$.
* (c) Find element forces.

---

**Problem 3 — Three-Bar Truss (Axial only)**
A triangular truss has nodes at coordinates:

* Node 1: (0, 0) — fixed
* Node 2: (3 m, 0) — roller (vertical displacement free)
* Node 3: (3 m, 4 m) — loaded with $40 \ \mathrm{kN}$ downwards

Members: (1–2), (2–3), (1–3), all with $E = 210 \ \mathrm{GPa}$, $A = 250 \ \mathrm{mm}^2$.

* (a) Form transformation matrices for each member.
* (b) Assemble the global stiffness matrix.
* (c) Solve for all displacements and element forces.

---

**Tip for Students:** Always draw:

1. Node/element diagram with DOFs
2. Connectivity table
3. Step-by-step stiffness assembly table

---

## **Nodal Equilibrium Equations**

Nodal equilibrium equations form the mathematical core of the finite element method. They express the condition that, at each node of a discretized structure, the algebraic sum of all forces (external and internal) must be zero for the system to be in static equilibrium.

### 1. Concept and Physical Meaning

In structural mechanics, equilibrium means that the **sum of forces and moments** acting on a body (or a node) is zero.
For a finite element model, this equilibrium condition is applied **at discrete nodal points**.

* **Internal forces** come from the deformation of connected elements (given by stiffness × displacement).
* **External forces** are the loads applied at the nodes (point loads, equivalent nodal loads from distributed forces, thermal effects, etc.).

At each free degree of freedom (DOF), equilibrium is expressed as:

$$
\sum F_{\text{internal}} + \sum F_{\text{external}} = 0
$$

Or, in FEM matrix form:

$$
[K] \{u\} = \{F\}
$$

Where:

* $[K]$ = global stiffness matrix (assembled from element stiffness matrices)
* $\{u\}$ = vector of unknown nodal displacements
* $\{F\}$ = vector of known external nodal loads

*(Insert Figure: Node connected to multiple elements showing internal and external forces — from Hutton Fig. 2.xx)*

### 2. Mathematical Formulation

For a single 1D bar element between nodes $i$ and $j$:

Element stiffness equation:

$$
\begin{bmatrix}
f_i \\
f_j
\end{bmatrix}
=
\frac{EA}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
\begin{bmatrix}
u_i \\
u_j
\end{bmatrix}
$$

Here, $f_i$ and $f_j$ are **internal nodal forces** — they are not applied loads but the forces the element exerts on its nodes due to deformation.

When several elements meet at a node, their contributions are summed algebraically:

$$
F_{\text{ext},k} = \sum_{e \in \text{connected to node k}} f_k^{(e)}
$$

This yields the **global nodal equilibrium equation** for node $k$.

In compact matrix form for the whole structure:

$$
[K] \{u\} = \{F\}
$$

Boundary conditions (prescribed displacements) are then applied to reduce and solve the system.

*(Insert Figure: Global equilibrium at a node showing multiple connecting elements — from Hutton Fig. 2.xx)*

### 3. Connection with DSM and MPE

* **Direct Stiffness Method (DSM)** assembles the $[K]$ matrix directly from element stiffnesses, then applies nodal equilibrium.
* **Minimum Potential Energy Principle (MPE)** arrives at the same equations by minimizing total potential energy.
* Both lead to the same governing equation $[K]\{u\} = \{F\}$, but via different reasoning — **force balance** vs. **energy minimization**.

### 4. Simple Numerical Illustration

Consider two bar elements in series: nodes 1–2–3, with stiffnesses $k_1$ and $k_2$. Node 1 is fixed, a load $P$ is applied at node 3.

Element stiffness matrices:

$$
[k^{(1)}] =
\begin{bmatrix}
k_1 & -k_1 \\
-k_1 & k_1
\end{bmatrix},
\quad
[k^{(2)}] =
\begin{bmatrix}
k_2 & -k_2 \\
-k_2 & k_2
\end{bmatrix}
$$

Global stiffness after assembly:

$$
[K] =
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1+k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
$$

Global equilibrium equation:

$$
[K] \{u_1, u_2, u_3\}^T = \{0, 0, P\}^T
$$

Apply BC: $u_1 = 0$, solve for $u_2, u_3$.

*(Insert Figure: Two-element bar system with loads and supports — from Hutton Fig. 2.xx)*

### 5. Key Points

* Nodal equilibrium is simply **ΣF = 0 applied at discrete points**.
* The FEM equation $[K]\{u\} = \{F\}$ is the algebraic form of nodal equilibrium.
* Internal forces are not the same as applied loads — they arise from deformation.
* Proper **assembly** and **sign conventions** are essential for correct results.
* This concept is valid for any structural type — trusses, beams, frames, solids.

---

Alright — here’s the **summary table** and an **ASCII “at a glance” diagram** that will drop cleanly into your `.md` file without breaking MathJax or GitHub rendering.

---

## **Nodal Equilibrium Equations — Summary Table**

| **Term**                 | **Meaning**                                      | **Expression**                                            |
| ------------------------ | ------------------------------------------------ | --------------------------------------------------------- |
| **Nodal equilibrium**    | Force balance at each node                       | $\sum F_{\text{internal}} + \sum F_{\text{external}} = 0$ |
| **Internal nodal force** | Force developed in an element due to deformation | $\{f^{(e)}\} = [k]^{(e)} \{u^{(e)}\}$                     |
| **Global equilibrium**   | Force balance for all nodes                      | $[K]\{u\} = \{F\}$                                        |
| **Boundary conditions**  | Known displacements or reactions                 | Applied to modify $[K]$ and $\{F\}$                       |
| **Result of solving**    | Nodal displacements, element forces, reactions   | —                                                         |

---
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 500 420" style="max-width:100%; height:auto;" role="img" aria-labelledby="title desc">
  <title id="title">Nodal Equilibrium — At a Glance</title>
  <desc id="desc">Diagram showing node, internal forces, external loads, and equilibrium equations.</desc>

  <style>
    .box{fill:#fff;stroke:#222;stroke-width:2;rx:8;ry:8}
    .arrow{stroke:#222;stroke-width:2;marker-end:url(#arrow)}
    .text{font-family:system-ui,Segoe UI,Helvetica,Arial,sans-serif;font-size:16px;fill:#111}
    .head{font-weight:700;font-size:18px}
    .math{font-family:ui-monospace,Consolas,Monaco,monospace}
  </style>

  <defs>
    <marker id="arrow" markerWidth="12" markerHeight="12" refX="10" refY="6" orient="auto">
      <path d="M0,0 L12,6 L0,12 z" fill="#222"/>
    </marker>
  </defs>

  <!-- Title -->
  <text class="head" x="250" y="30" text-anchor="middle">Nodal Equilibrium — At a Glance</text>

  <!-- Node -->
  <rect class="box" x="180" y="60" width="140" height="60"/>
  <text class="text" x="250" y="95" text-anchor="middle">Node k</text>

  <!-- Arrow down -->
  <line class="arrow" x1="250" y1="120" x2="250" y2="150"/>

  <!-- Left (Internal Forces) -->
  <text class="text" x="80" y="170" text-anchor="middle">Internal Forces</text>
  <text class="text" x="80" y="190" text-anchor="middle">from all</text>
  <text class="text" x="80" y="210" text-anchor="middle">connected elements</text>
  <line class="arrow" x1="120" y1="180" x2="200" y2="180"/>

  <!-- Right (External Loads) -->
  <text class="text" x="420" y="170" text-anchor="middle">External Loads</text>
  <text class="text" x="420" y="190" text-anchor="middle">(Given)</text>
  <line class="arrow" x1="300" y1="180" x2="380" y2="180"/>

  <!-- Middle Equation -->
  <text class="math" x="250" y="250" text-anchor="middle">ΣF_internal + ΣF_external = 0</text>

  <!-- Arrow down -->
  <line class="arrow" x1="250" y1="260" x2="250" y2="290"/>

  <!-- Final Equation -->
  <text class="math" x="250" y="330" text-anchor="middle">[K]{u} = {F}</text>
</svg>

---

**Figure Placeholders (from Hutton’s book):**

* *(Insert Figure: Free-body diagram of a node with connected element forces — Hutton Fig. 2.xx)*
* *(Insert Figure: Assembled nodal equilibrium equations for a multi-element system — Hutton Fig. 2.xx)*

---

## **Assembly of Global Stiffness Matrix**

The **global stiffness matrix** $[K]$ represents the stiffness relationship for the entire structure, assembled from the **local stiffness matrices** $[k]^{(e)}$ of individual elements. It allows us to relate all nodal displacements to all applied nodal loads in a single system of equations.

$$
[K] \{u\} = \{F\}
$$

Where:

* $[K]$ = global stiffness matrix (size: total DOFs × total DOFs)
* $\{u\}$ = global nodal displacement vector
* $\{F\}$ = global nodal force vector

---

### 1. Concept

Each element has its own **local stiffness matrix** defined for its own node numbering (local coordinates). To analyze the entire structure:

1. **Map local DOFs to global DOFs** using an **element connectivity table**.
2. **Place each local stiffness matrix** into the correct position within the global matrix, adding contributions if multiple elements share a DOF.

*(Insert Figure: Example showing element local node numbering and global node numbering — from Hutton Fig. 2.xx)*

---

### 2. Assembly Procedure

**Step 1 — Number the nodes**
Assign a unique global DOF number to each displacement variable in the structure.

**Step 2 — Prepare the connectivity table**
The table lists, for each element, the correspondence between local node numbers and global DOF numbers.

| Element | Local Node 1 | Local Node 2 | Global DOF 1 | Global DOF 2 |
| ------- | ------------ | ------------ | ------------ | ------------ |
| 1       | 1            | 2            | 1            | 2            |
| 2       | 2            | 3            | 2            | 3            |

**Step 3 — Form each element stiffness matrix** in local coordinates.

For a 1D bar element:

$$
[k]^{(e)} = \frac{EA}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

**Step 4 — Insert local matrices into global matrix** according to the connectivity table.

If an element’s local DOFs correspond to global DOFs $i$ and $j$, place:

* $k_{11}$ into $[K]_{ii}$
* $k_{12}$ into $[K]_{ij}$
* $k_{21}$ into $[K]_{ji}$
* $k_{22}$ into $[K]_{jj}$

Add contributions if another element already has entries in those locations.

**Step 5 — Continue until all elements are assembled** into $[K]$.

*(Insert Figure: Bar elements in series and parallel, showing stiffness contribution in the global matrix — from Hutton Fig. 2.xx)*

---

### 3. Example — Two Bar Elements in Series

**Given:**

* Element 1: Nodes 1–2, stiffness $k_1$
* Element 2: Nodes 2–3, stiffness $k_2$

**Element stiffness matrices:**

$$
[k]^{(1)} =
\begin{bmatrix}
k_1 & -k_1 \\
-k_1 & k_1
\end{bmatrix}
\quad
[k]^{(2)} =
\begin{bmatrix}
k_2 & -k_2 \\
-k_2 & k_2
\end{bmatrix}
$$

**Global stiffness matrix assembly:**

* From $k^{(1)}$:
  $[K]_{11} += k_1,\ [K]_{12} += -k_1,\ [K]_{21} += -k_1,\ [K]_{22} += k_1$
* From $k^{(2)}$:
  $[K]_{22} += k_2,\ [K]_{23} += -k_2,\ [K]_{32} += -k_2,\ [K]_{33} += k_2$

Final global stiffness matrix:

$$
[K] =
\begin{bmatrix}
k_1 & -k_1 & 0 \\
-k_1 & k_1 + k_2 & -k_2 \\
0 & -k_2 & k_2
\end{bmatrix}
$$

---

### 4. Key Points

* Assembly is **additive** — stiffness contributions from different elements connected at a node are summed.
* The **size of $[K]$** equals the total number of global DOFs.
* **Connectivity tables** are essential for mapping local DOFs to global DOFs.
* After assembly, $[K]$ is **symmetric** for linear elastic problems.
* Boundary conditions are applied after the full global matrix is assembled.

---

## **Assembly of Global Stiffness Matrix — Summary Table**

| **Step**                     | **Description**                                                                 | **Key Formula / Note**                                                    |
| ---------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| 1. Node numbering            | Assign unique global DOF numbers to all displacement variables                  | —                                                                         |
| 2. Connectivity table        | Map element local nodes to global DOFs                                          | Essential for correct placement                                           |
| 3. Local stiffness matrix    | Compute $[k]^{(e)}$ for each element in local coordinates                       | For 1D bar: $\frac{EA}{L} \begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}$ |
| 4. Insert into global matrix | Place local stiffness terms into $[K]$ at positions given by connectivity table | Add contributions if entries already exist                                |
| 5. Complete $[K]$            | Repeat for all elements until global stiffness is fully assembled               | $[K]$ is symmetric for linear elastic problems                            |
| 6. Apply BCs                 | Impose known displacements or forces                                            | Modify $[K]$ and $\{F\}$ before solving                                   |

**Key Points:**

* Assembly is **additive** at shared DOFs.
* Global matrix size = total global DOFs × total global DOFs.
* Symmetry of $[K]$ simplifies computation.
* Assembly is independent of the solution method — it’s purely a bookkeeping step.

---

## **Element Strain and Stress**

Once the nodal displacements are determined from

$$
[K]\{u\} = \{F\}
$$

the next step is to compute **element strains** and **stresses**. These are essential for checking the structural performance against strength and serviceability requirements.

---

### 1. Concept

* **Strain** is a measure of deformation — how much an element elongates or contracts relative to its original length.
* **Stress** is the internal force per unit area developed in the material to resist deformation.

The displacement solution $\{u\}$ from the FEM analysis provides all the information needed to calculate these quantities for each element.

*(Insert Figure: 1D bar element showing original length $L$, cross-section $A$, nodal displacements $u_1, u_2$ — Hutton Fig. 2.xx)*

---

### 2. Strain–Displacement Relation (1D Bar)

For a 1D bar element between nodes $1$ and $2$:

$$
\varepsilon = \frac{\Delta L}{L} = \frac{u_2 - u_1}{L}
$$

In **matrix form**:

$$
\varepsilon = [B] \{u_e\}
$$

where:

$$
[B] = \begin{bmatrix} -\frac{1}{L} & \frac{1}{L} \end{bmatrix}
$$

and:

$$
\{u_e\} =
\begin{bmatrix}
u_1 \\
u_2
\end{bmatrix}
$$

*(Insert Figure: Diagram illustrating element elongation and sign convention for tensile strain — Hutton Fig. 2.xx)*

---

### 3. Stress–Strain Relation

Using **Hooke’s law** for a linearly elastic material:

$$
\sigma = E \cdot \varepsilon
$$

Substitute $\varepsilon$ from above:

$$
\sigma = E \cdot \frac{u_2 - u_1}{L}
$$

In matrix form:

$$
\sigma = [D] \, [B] \, \{u_e\}
$$

where:

$$
[D] = [E] \quad \text{(scalar in 1D)}
$$

*(Insert Figure: Stress distribution in a bar element under tension — Hutton Fig. 2.xx)*

---

### 4. Internal Element Force

The **axial force** in the element is:

$$
F_{\text{int}} = \sigma \cdot A
= E \cdot A \cdot \frac{u_2 - u_1}{L}
$$

In matrix form:

$$
\{f_e\} =
[k] \{u_e\}
$$

where $[k] = \frac{EA}{L} \begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}$.

*(Insert Figure: Axial force diagram for a bar element — Hutton Fig. 2.xx)*

---

### 5. Generalization to Multiple DOFs

For **multi-DOF or 2D/3D elements**, the same basic steps apply:

1. Extract the element displacement vector $\{u_e\}$ from the global solution $\{u\}$ using connectivity.
2. Compute strains using:

$$
\{\varepsilon\} = [B] \{u_e\}
$$

where $[B]$ is the **strain–displacement matrix** for the element.
3\. Compute stresses using:

$$
\{\sigma\} = [D] \{\varepsilon\}
$$

where $[D]$ is the **material property matrix**.

---

### 6. Worked Mini-Example (1D Bar)

**Given:**

* $E = 210 \ \text{GPa}$
* $A = 300 \ \text{mm}^2$
* $L = 2.0 \ \text{m}$
* $u_1 = 0$, $u_2 = 0.001 \ \text{m}$

**Step 1 — Strain:**

$$
\varepsilon = \frac{0.001 - 0}{2.0} = 0.0005
$$

**Step 2 — Stress:**

$$
\sigma = (210 \times 10^9) (0.0005) = 105 \ \text{MPa}
$$

**Step 3 — Internal Force:**

$$
F_{\text{int}} = 105 \times 10^6 \times 300 \times 10^{-6} = 31.5 \ \text{kN}
$$

---

### 7. Key Points

* Displacements → Strains → Stresses → Internal Forces.
* $[B]$ matrix links displacements to strains; $[D]$ matrix links strains to stresses.
* Sign conventions are critical (tension = positive strain).
* Post-processing in FEM is where physical performance checks happen.
* The accuracy of strain/stress results depends heavily on **mesh refinement** and **element type**.

---

## **Element Strain and Stress — Summary Table**

| **Step**                    | **Formula**                                                                                      | **Notes**                            |
| --------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------ |
| **1. Strain**               | $\varepsilon = \frac{u_2 - u_1}{L}$                                                              | 1D bar; sign convention: tension $+$ |
| **2. Matrix form (strain)** | $\varepsilon = [B] \{u_e\},\quad [B] = \begin{bmatrix} -\frac{1}{L} & \frac{1}{L} \end{bmatrix}$ | $[B]$ = strain–displacement matrix   |
| **3. Stress**               | $\sigma = E \cdot \varepsilon$                                                                   | Hooke’s law (linear elasticity)      |
| **4. Matrix form (stress)** | $\sigma = [D][B]\{u_e\}$                                                                         | $[D] = E$ in 1D                      |
| **5. Internal force**       | $F_{\text{int}} = \sigma \cdot A = E A \frac{u_2 - u_1}{L}$                                      | Also from $[k]\{u_e\}$               |
| **6. Generalization**       | $\{\varepsilon\} = [B]\{u_e\}, \quad \{\sigma\} = [D]\{\varepsilon\}$                            | Works for multi-DOF 2D/3D elements   |

**Key Points:**

* Displacement solution drives all strain/stress calculations.
* Use correct element $[B]$ and $[D]$ matrices.
* FEM post-processing checks performance against material limits.

---







