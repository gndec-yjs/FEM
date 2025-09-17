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

## Direct Stiffness Method

#### 1. Introduction

The simple line elements discussed earlier introduced the concepts of **nodes, nodal displacements, and element stiffness matrices**.  
In this section, we extend these ideas to create a **finite element model of a structure** composed of many elements.

We focus on **truss structures**, defined as assemblies of straight elastic members connected by pin joints, subjected only to **axial forces**.  
Even though the bar element is inherently one-dimensional, it can effectively be used in analyzing both **2D** and **3D trusses**.


#### 2. Global Coordinate System

The **global coordinate system (X–Y or X–Y–Z)** is the reference frame in which structural displacements are expressed. It is usually chosen for convenience, aligned with the geometry of the structure.

<img width="1033" height="386" alt="image" src="https://github.com/user-attachments/assets/e33b41e0-6056-44b8-9a1e-1dfdbde387a2" />

Consider the **cantilever truss** in Figure (3.1 a). If we isolate a **single joint** (Figure 3.1 b), we observe:

- Multiple element nodes connect at a single global node.  
- The local **element axes (x)** generally do not align with the global axes (X, Y).  

This leads to key FEM premises:

1. **Displacement compatibility**:  
   The displacement of each connected element at a joint must equal the global nodal displacement.

2. **Transformation requirement**:  
   Each element’s stiffness matrix and forces must be transformed from **local element coordinates** to the **global coordinate system** for consistent assembly.

3. **Post-processing**:  
   After solving for nodal displacements in the global system, element stresses and strains are obtained by transforming results back to the element’s local coordinates.

#### 3. Why Base the Formulation on Displacements?

In engineering practice, the **final quantity of interest is stress**, since stresses are compared with material strength (e.g., yield stress).  
However, it is **easier to prescribe external loads** than to predict displacements.  

Therefore:
- The FEM equations are formulated in terms of **nodal displacements**.  
- From nodal displacements, **strains** and then **stresses** are computed by back-substitution.  

This is the essence of the **stiffness method**.

In contrast, the **flexibility method** takes forces as primary unknowns and computes displacements afterwards. But displacement compatibility at nodes becomes algebraically tedious.  
Thus, FEM universally prefers the **stiffness (displacement-based) approach**.

#### 4. Assembly in the Direct Stiffness Method

Returning to Figure 3.1b, where multiple elements meet at a node:

- Elements contribute stiffness to the global system depending on their **orientation**.  
- Example (by intuition):  
  - Elements aligned with the X axis → stiffness contribution in X direction only.  
  - Elements aligned with the Y axis → stiffness in Y direction only.  
  - Inclined elements → contributions in both X and Y directions.

The **Direct Stiffness Method** involves:

1. Transforming each element stiffness matrix into the global coordinate system.  
2. Assembling all transformed element matrices directly into the **global stiffness matrix**, using element connectivity.  

This ensures:
- **Displacement compatibility** at nodes is automatically enforced.  
- The assembled global system is equivalent to what would be obtained by a formal equilibrium approach (joint equilibrium equations), but simpler to handle computationally.  

#### 5. Key Advantage

- The **Direct Stiffness Method** guarantees compatibility at connections.  
- Stresses are obtained by back-substitution after solving for displacements.  
- It is systematic and lends itself to **automation in computer programs**.

---

## Nodal Equilibrium Equations

To illustrate how element properties are converted to the global coordinate system,  
consider a **1D bar element** as a structural member of a **2D truss**.  

This simple example demonstrates the **general FEM assembly procedure**:

1. Select the element type (here: bar element).  
2. Define geometry and element connectivity.  
3. Formulate governing equations (static equilibrium).  
4. Apply **boundary conditions** (supports and external forces).  
5. Solve for **global nodal displacements**.  
6. Back-substitute to compute **secondary variables** (strain, stress, reactions).  

> 🔎 *Note:* In FEM terminology, strain and stress are called **secondary variables** only because they are computed after displacement solution. In design, they are of **primary importance**.

#### Two-Element Truss Example

Consider the **two-element truss** shown below:

- Nodes are numbered (circled).  
- Elements are numbered (boxed).  
- Global displacements use the convention:  
  - $U_{2i-1}$ → displacement of node *i* in global X.  
  - $U_{2i}$ → displacement of node *i* in global Y.  

<img width="1058" height="590" alt="image" src="https://github.com/user-attachments/assets/8900e111-401a-47db-b74d-8a2b34e1d621" />


#### Free-Body Equilibrium at Nodes

<img width="925" height="758" alt="image" src="https://github.com/user-attachments/assets/924e5d09-896c-491d-ba62-e162f1547d5d" />

Writing equilibrium at each node (Figure 3.3):

**Node 1:**
$$
F_1 - f_1^{(1)} \cos\theta_1 = 0
\tag{3.1a}
$$

$$
F_2 - f_1^{(1)} \sin\theta_1 = 0
\tag{3.1b}
$$

**Node 2:**
$$
F_3 - f_2^{(2)} \cos\theta_2 = 0
\tag{3.2a}
$$

$$
F_4 - f_2^{(2)} \sin\theta_2 = 0
\tag{3.2b}
$$

**Node 3:**
$$
F_5 - f_3^{(1)} \cos\theta_1 - f_3^{(2)} \cos\theta_2 = 0
\tag{3.3a}
$$

$$
F_6 - f_3^{(1)} \sin\theta_1 - f_3^{(2)} \sin\theta_2 = 0
\tag{3.3b}
$$

#### Element Displacements and Transformation

<img width="1149" height="524" alt="image" src="https://github.com/user-attachments/assets/daef1cad-fdeb-406f-b9b0-8280252f4d70" />

Consider a bar element oriented at angle $\theta$ (Figure 3.4):

- Local element displacements ($u_1^{(e)}, v_1^{(e)}, u_2^{(e)}, v_2^{(e)}$).  
- Global displacements ($U_1^{(e)}, U_2^{(e)}, U_3^{(e)}, U_4^{(e)}$).  

Relation between **local** and **global** displacements:

$$
u_1^{(e)} = U_1^{(e)} \cos\theta + U_2^{(e)} \sin\theta
\tag{3.4a}
$$

$$
v_1^{(e)} = -U_1^{(e)} \sin\theta + U_2^{(e)} \cos\theta
\tag{3.4b}
$$

$$
u_2^{(e)} = U_3^{(e)} \cos\theta + U_4^{(e)} \sin\theta
\tag{3.4c}
$$

$$
v_2^{(e)} = -U_3^{(e)} \sin\theta + U_4^{(e)} \cos\theta
\tag{3.4d}
$$

#### Axial Deformation and Element Force

The axial deformation of element $e$ is:

$$
\delta^{(e)} = u_2^{(e)} - u_1^{(e)}
= (U_3^{(e)} - U_1^{(e)}) \cos\theta + (U_4^{(e)} - U_2^{(e)}) \sin\theta
\tag{3.5}
$$

The corresponding axial force:

$$
f^{(e)} = k^{(e)} \, \delta^{(e)}
\tag{3.6}
$$

#### Element Forces for 2-Bar Truss

For **Element 1** (nodes 1–3):

$$
f_3^{(1)} = -f_1^{(1)} =
k^{(1)}\Big[(U_5 - U_1)\cos\theta_1 + (U_6 - U_2)\sin\theta_1\Big]
\tag{3.7}
$$

For **Element 2** (nodes 2–3):

$$
f_3^{(2)} = -f_2^{(2)} =
k^{(2)}\Big[(U_5 - U_3)\cos\theta_2 + (U_6 - U_4)\sin\theta_2\Big]
\tag{3.8}
$$

#### Substituting into Nodal Equilibrium

Now substituting Eqs. (3.7) and (3.8) into equilibrium equations (3.1–3.3):

$$
-k^{(1)}\Big[(U_5-U_1)\cos\theta_1 + (U_6-U_2)\sin\theta_1\Big]\cos\theta_1 = F_1
\tag{3.9}
$$

$$
-k^{(1)}\Big[(U_5-U_1)\cos\theta_1 + (U_6-U_2)\sin\theta_1\Big]\sin\theta_1 = F_2
\tag{3.10}
$$

$$
-k^{(2)}\Big[(U_5-U_3)\cos\theta_2 + (U_6-U_4)\sin\theta_2\Big]\cos\theta_2 = F_3
\tag{3.11}
$$

$$
-k^{(2)}\Big[(U_5-U_3)\cos\theta_2 + (U_6-U_4)\sin\theta_2\Big]\sin\theta_2 = F_4
\tag{3.12}
$$

$$
k^{(1)}\Big[(U_5-U_1)\cos\theta_1 + (U_6-U_2)\sin\theta_1\Big]\cos\theta_1 +
k^{(2)}\Big[(U_5-U_3)\cos\theta_2 + (U_6-U_4)\sin\theta_2\Big]\cos\theta_2 = F_5
\tag{3.13}
$$

$$
k^{(1)}\Big[(U_5-U_1)\cos\theta_1 + (U_6-U_2)\sin\theta_1\Big]\sin\theta_1 +
k^{(2)}\Big[(U_5-U_3)\cos\theta_2 + (U_6-U_4)\sin\theta_2\Big]\sin\theta_2 = F_6
\tag{3.14}
$$


#### Matrix Form

The six equilibrium equations can be written compactly as:

$$
[K]\{U\} = \{F\}
\tag{3.15}
$$

where:  
- $[K]$ = global stiffness matrix (symmetric, $6\times6$).  
- $\{U\}$ = global displacement vector.  
- $\{F\}$ = applied nodal force vector.  

This is the standard FEM system of equations:

$$
[K]\{U\} = \{F\}
\tag{3.16}
$$

Application of boundary conditions and solution steps follow in subsequent sections.

---

### Element Transformation

In the previous section, we obtained the global equilibrium equations by **directly writing nodal equilibrium conditions**.  
However, this approach becomes **cumbersome** for systems with many elements.  

A more systematic method is to transform each **element stiffness matrix** from the **local element coordinates** to the **global coordinate system**.  
This allows for direct assembly of the global stiffness matrix.

#### Local Element Stiffness Equations

For a bar element in its **local (element) coordinate system**, the equilibrium equations are:

$$
\begin{bmatrix}
k_e & -k_e \\
-k_e & k_e
\end{bmatrix}
\begin{Bmatrix}
u_1^{(e)} \\
u_2^{(e)}
\end{Bmatrix}
=
\begin{Bmatrix}
f_1^{(e)} \\
f_2^{(e)}
\end{Bmatrix}
\tag{3.17}
$$

where:

- $k_e = \dfrac{AE}{L}$ = axial stiffness of the element  
- $u_1^{(e)}, u_2^{(e)}$ = element nodal displacements (local coordinates)  
- $f_1^{(e)}, f_2^{(e)}$ = element nodal forces (local coordinates)  

#### Target Form in Global Coordinates

We want to express the equilibrium equations in the **global coordinate system** as:

$$
[K^{(e)}]
\begin{Bmatrix}
U_1^{(e)} \\ U_2^{(e)} \\ U_3^{(e)} \\ U_4^{(e)}
\end{Bmatrix}
=
\begin{Bmatrix}
F_1^{(e)} \\ F_2^{(e)} \\ F_3^{(e)} \\ F_4^{(e)}
\end{Bmatrix}
\tag{3.18}
$$

Here:

- $[K^{(e)}]$ = element stiffness matrix in **global coordinates**  
- $\{U^{(e)}\}$ = element nodal displacements in **global X, Y** directions  
- $\{F^{(e)}\}$ = element nodal forces in **global X, Y** directions  

#### Transformation of Displacements

From geometry (see Figure 3.4), the relation between **local axial displacements** and **global displacements** is:

$$
u_1^{(e)} = U_1^{(e)} \cos\theta + U_2^{(e)} \sin\theta
\tag{3.19}
$$

$$
u_2^{(e)} = U_3^{(e)} \cos\theta + U_4^{(e)} \sin\theta
\tag{3.20}
$$

In **matrix form**:

$$
\begin{Bmatrix}
u_1^{(e)} \\ u_2^{(e)}
\end{Bmatrix}
=
[R]
\begin{Bmatrix}
U_1^{(e)} \\ U_2^{(e)} \\ U_3^{(e)} \\ U_4^{(e)}
\end{Bmatrix}
\tag{3.21}
$$

where the **transformation matrix** $[R]$ is:

$$
[R] =
\begin{bmatrix}
\cos\theta & \sin\theta & 0 & 0 \\
0 & 0 & \cos\theta & \sin\theta
\end{bmatrix}
\tag{3.22}
$$

#### Transformation of Forces and Stiffness

Substituting Eq. (3.21) into Eq. (3.17):

$$
[k^{(e)}][R]
\begin{Bmatrix}
U_1^{(e)} \\ U_2^{(e)} \\ U_3^{(e)} \\ U_4^{(e)}
\end{Bmatrix}
=
\begin{Bmatrix}
f_1^{(e)} \\ f_2^{(e)}
\end{Bmatrix}
\tag{3.23}
$$

Premultiplying by $[R]^T$:

$$
[R]^T [k^{(e)}][R]
\begin{Bmatrix}
U_1^{(e)} \\ U_2^{(e)} \\ U_3^{(e)} \\ U_4^{(e)}
\end{Bmatrix}
=
\begin{Bmatrix}
F_1^{(e)} \\ F_2^{(e)} \\ F_3^{(e)} \\ F_4^{(e)}
\end{Bmatrix}
\tag{3.26}
$$

Thus, the **global element stiffness matrix** is:

$$
[K^{(e)}] = [R]^T [k^{(e)}] [R]
\tag{3.27}
$$

#### Expanded Form of Global Stiffness Matrix

With $c = \cos\theta$, $s = \sin\theta$, we obtain:

$$
[K^{(e)}] = k_e
\begin{bmatrix}
c^2 & cs & -c^2 & -cs \\
cs & s^2 & -cs & -s^2 \\
-c^2 & -cs & c^2 & cs \\
-cs & -s^2 & cs & s^2
\end{bmatrix}
\tag{3.28}
$$

#### Direction Cosines

For an element connecting nodes $i$ and $j$ with global coordinates:

- Node $i: (X_i, Y_i)$  
- Node $j: (X_j, Y_j)$  

The **element length** is:

$$
L = \sqrt{(X_j - X_i)^2 + (Y_j - Y_i)^2}
\tag{3.29}
$$

The **unit vector** along the element is:

$$
\vec{l} = \dfrac{1}{L} \big[(X_j - X_i)\mathbf{i} + (Y_j - Y_i)\mathbf{j}\big]
\tag{3.30}
$$

The **direction cosines** are:

$$
\cos\theta = \dfrac{X_j - X_i}{L}
\tag{3.31}
$$

$$
\sin\theta = \dfrac{Y_j - Y_i}{L}
\tag{3.32}
$$

Thus, the **global element stiffness matrix** (Eq. 3.28) can be constructed **directly from nodal coordinates**, along with $A$, $E$, and $L$.

---

## Assembly of Global Stiffness Matrix

Having derived how to transform the element stiffness matrix into the **global coordinate system**, we now describe the **direct assembly procedure**.  
This method constructs the **global stiffness matrix** element by element, using **connectivity relations**.

#### Step 1: Element Stiffness Matrices in Global Coordinates

For a two-element truss (Figure 3.2), the **global element stiffness matrices** are obtained using Equation (3.28).  

For **element 1**:

$$
[K^{(1)}] =
\begin{bmatrix}
k^{(1)}_{11} & k^{(1)}_{12} & k^{(1)}_{13} & k^{(1)}_{14} \\
k^{(1)}_{21} & k^{(1)}_{22} & k^{(1)}_{23} & k^{(1)}_{24} \\
k^{(1)}_{31} & k^{(1)}_{32} & k^{(1)}_{33} & k^{(1)}_{34} \\
k^{(1)}_{41} & k^{(1)}_{42} & k^{(1)}_{43} & k^{(1)}_{44}
\end{bmatrix}
\tag{3.33}
$$

For **element 2**:

$$
[K^{(2)}] =
\begin{bmatrix}
k^{(2)}_{11} & k^{(2)}_{12} & k^{(2)}_{13} & k^{(2)}_{14} \\
k^{(2)}_{21} & k^{(2)}_{22} & k^{(2)}_{23} & k^{(2)}_{24} \\
k^{(2)}_{31} & k^{(2)}_{32} & k^{(2)}_{33} & k^{(2)}_{34} \\
k^{(2)}_{41} & k^{(2)}_{42} & k^{(2)}_{43} & k^{(2)}_{44}
\end{bmatrix}
\tag{3.34}
$$

#### Step 2: Connectivity Relations

Each **element displacement vector** must be expressed in terms of the **global displacement vector**.

For **element 1**:

$$
\{U^{(1)}\} =
\begin{Bmatrix}
U^{(1)}_1 \\ U^{(1)}_2 \\ U^{(1)}_3 \\ U^{(1)}_4
\end{Bmatrix}
\;\;\Rightarrow\;\;
\begin{Bmatrix}
U_1 \\ U_2 \\ U_5 \\ U_6
\end{Bmatrix}
\tag{3.35}
$$

For **element 2**:

$$
\{U^{(2)}\} =
\begin{Bmatrix}
U^{(2)}_1 \\ U^{(2)}_2 \\ U^{(2)}_3 \\ U^{(2)}_4
\end{Bmatrix}
\;\;\Rightarrow\;\;
\begin{Bmatrix}
U_3 \\ U_4 \\ U_5 \\ U_6
\end{Bmatrix}
\tag{3.36}
$$

These equations describe **element-to-global displacement mapping**.

#### Step 3: Nodal Displacement Correspondence Table

To organize connectivity, we use a **correspondence table**:

| **Global Displacement** | **Element 1 Displacement** | **Element 2 Displacement** |
| ------------------------ | -------------------------- | -------------------------- |
| $U_1$                   | $u^{(1)}_1$               | 0                          |
| $U_2$                   | $u^{(1)}_2$               | 0                          |
| $U_3$                   | 0                         | $u^{(2)}_1$               |
| $U_4$                   | 0                         | $u^{(2)}_2$               |
| $U_5$                   | $u^{(1)}_3$               | $u^{(2)}_3$               |
| $U_6$                   | $u^{(1)}_4$               | $u^{(2)}_4$               |

**Interpretation:**  
- A **zero entry** means that the element does not contribute stiffness for that displacement.  
- For example, element 1 has no contribution to $U_3$ and $U_4$, since it is not connected to global node 2.  

#### Step 4: Assembly of Global Stiffness Matrix

Using the correspondence, the **global stiffness matrix** is assembled term by term:

- $$K_{11} = k^{(1)}_{11} + 0$$
- $$K_{12} = k^{(1)}_{12} + 0$$
- $$K_{13} = 0 + 0$$
- $$K_{14} = 0 + 0$$
- $$K_{15} = k^{(1)}_{13} + 0$$
- $$K_{16} = k^{(1)}_{14} + 0$$

- $$K_{22} = k^{(1)}_{22} + 0$$
- $$K_{23} = 0 + 0$$
- $$K_{24} = 0 + 0$$
- $$K_{25} = k^{(1)}_{23} + 0$$
- $$K_{26} = k^{(1)}_{24} + 0$$

- $$K_{33} = 0 + k^{(2)}_{11}$$
- $$K_{34} = 0 + k^{(2)}_{12}$$
- $$K_{35} = 0 + k^{(2)}_{13}$$
- $$K_{36} = 0 + k^{(2)}_{14}$$

- $$K_{44} = 0 + k^{(2)}_{22}$$
- $$K_{45} = 0 + k^{(2)}_{23}$$
- $$K_{46} = 0 + k^{(2)}_{24}$$

- $$K_{55} = k^{(1)}_{33} + k^{(2)}_{33}$$
- $$K_{56} = k^{(1)}_{34} + k^{(2)}_{34}$$
- $$K_{66} = k^{(1)}_{44} + k^{(2)}_{44}$$


The **symmetry** of the stiffness matrix has been used to avoid repetition.


#### Step 5: Global Stiffness Matrix

The final **global stiffness matrix** $[K]$ is identical to that obtained earlier by the equilibrium method (Section 3.2).  

This confirms the **Direct Stiffness Method**:  

- Transform each element stiffness matrix to the global system  
- Assemble them using connectivity relations  
- Form the complete global stiffness matrix  

$$
[K]\{U\} = \{F\}
$$

where  

- $[K]$ = global stiffness matrix  
- $\{U\}$ = global nodal displacement vector  
- $\{F\}$ = global nodal force vector  

---

### Example 3.1: Assembly of Global Stiffness Matrix for a Two-Element Truss

**Problem Statement:**  
Consider the two-element truss shown in Figure 3.2.  
- Element 1 is oriented at $\theta_1 = \pi/4$  
- Element 2 is oriented at $\theta_2 = 0$  
- The element stiffness values are:  
  $$
  k_1 = \frac{A_1 E_1}{L_1}, 
  \quad
  k_2 = \frac{A_2 E_2}{L_2}
  $$
  
<img width="1010" height="567" alt="image" src="https://github.com/user-attachments/assets/04de1061-9be3-46b0-a6d9-f4c062053960" />

Transform the stiffness matrix of each element into the **global coordinate system** and assemble the **global stiffness matrix** for the complete truss.

---

### Solution:

#### Step 1 — Element 1: transform to global frame

For element 1, $$\theta_1 = \dfrac{\pi}{4}\Rightarrow
\cos\theta_1=\sin\theta_1=\dfrac{\sqrt{2}}{2}.$$  
Thus

$$
\cos^2\theta_1=\sin^2\theta_1=\cos\theta_1\sin\theta_1=\dfrac{1}{2}.
$$

Use the general transformed element stiffness (Eq. 3.28):

$$
[K^{(e)}] = k_e
\begin{bmatrix}
c^2 & cs & -c^2 & -cs \\
cs & s^2 & -cs & -s^2 \\
- c^2 & -cs & c^2 & cs \\
- cs & -s^2 & cs & s^2
\end{bmatrix}
$$

Substituting \(c=s=\dfrac{\sqrt{2}}{2}\) and \(k_e=k_1\) gives

$$
[K^{(1)}]
= k_1
\begin{bmatrix}
\tfrac12 & \tfrac12 & -\tfrac12 & -\tfrac12 \\
\tfrac12 & \tfrac12 & -\tfrac12 & -\tfrac12 \\
-\tfrac12 & -\tfrac12 & \tfrac12 & \tfrac12 \\
-\tfrac12 & -\tfrac12 & \tfrac12 & \tfrac12
\end{bmatrix}
=
\frac{k_1}{2}
\begin{bmatrix}
1 & 1 & -1 & -1 \\
1 & 1 & -1 & -1 \\
-1 & -1 & 1 & 1 \\
-1 & -1 & 1 & 1
\end{bmatrix}.
$$

#### Step 2 — Element 2: transform to global frame

For element 2, $$\theta_2 = 0\Rightarrow \cos\theta_2=1,\; \sin\theta_2=0.$$

Substitute into Eq. (3.28) with \(k_e=k_2\):

$$
[K^{(2)}]
= k_2
\begin{bmatrix}
1 & 0 & -1 & 0 \\
0 & 0 & 0 & 0 \\
-1 & 0 & 1 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}.
$$

#### Step 3 — Element ↔ Global displacement mapping (connectivity)

From the element-to-global mapping (Eqs. 3.35–3.36):

- Element 1 local displacement vector corresponds to global displacements:
  $$
  \{U^{(1)}\} \Rightarrow \{U_1,\,U_2,\,U_5,\,U_6\}
  $$
- Element 2 local displacement vector corresponds to global displacements:
  $$
  \{U^{(2)}\} \Rightarrow \{U_3,\,U_4,\,U_5,\,U_6\}
  $$

Use the correspondence table to place each element's stiffness terms into the appropriate positions of the global \(6\times6\) matrix.

#### Step 4 — Assemble each global \(K_{ij}\) (including zeros)

Let us list every assembled global stiffness entry \(K_{ij}\) (indices 1..6), including those that are zero.

$$
\begin{aligned}
K_{11} &= \tfrac{k_1}{2}, &\qquad K_{12} &= \tfrac{k_1}{2}, &\qquad K_{13} &= 0, &\qquad K_{14} &= 0, &\qquad K_{15} &= -\tfrac{k_1}{2}, &\qquad K_{16} &= -\tfrac{k_1}{2},\\[6pt]
K_{21} &= \tfrac{k_1}{2}, &\qquad K_{22} &= \tfrac{k_1}{2}, &\qquad K_{23} &= 0, &\qquad K_{24} &= 0, &\qquad K_{25} &= -\tfrac{k_1}{2}, &\qquad K_{26} &= -\tfrac{k_1}{2},\\[6pt]
K_{31} &= 0, &\qquad K_{32} &= 0, &\qquad K_{33} &= k_2, &\qquad K_{34} &= 0, &\qquad K_{35} &= -k_2, &\qquad K_{36} &= 0,\\[6pt]
K_{41} &= 0, &\qquad K_{42} &= 0, &\qquad K_{43} &= 0, &\qquad K_{44} &= 0, &\qquad K_{45} &= 0, &\qquad K_{46} &= 0,\\[6pt]
K_{51} &= -\tfrac{k_1}{2}, &\qquad K_{52} &= -\tfrac{k_1}{2}, &\qquad K_{53} &= -k_2, &\qquad K_{54} &= 0, &\qquad K_{55} &= \tfrac{k_1}{2} + k_2, &\qquad K_{56} &= \tfrac{k_1}{2},\\[6pt]
K_{61} &= -\tfrac{k_1}{2}, &\qquad K_{62} &= -\tfrac{k_1}{2}, &\qquad K_{63} &= 0, &\qquad K_{64} &= 0, &\qquad K_{65} &= \tfrac{k_1}{2}, &\qquad K_{66} &= \tfrac{k_1}{2}.
\end{aligned}
$$

*(Matrix symmetry implies \(K_{ij}=K_{ji}\); both upper and lower parts are listed for clarity.)*

#### Step 5 — Final global stiffness matrix

The assembled global stiffness matrix for the two-element truss is

$$
[K] =
\begin{bmatrix}
\; \tfrac{k_1}{2} & \; \tfrac{k_1}{2} & 0 & 0 & -\tfrac{k_1}{2} & -\tfrac{k_1}{2} \\
\; \tfrac{k_1}{2} & \; \tfrac{k_1}{2} & 0 & 0 & -\tfrac{k_1}{2} & -\tfrac{k_1}{2} \\
0 & 0 & k_2 & 0 & -k_2 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 \\
-\,\tfrac{k_1}{2} & -\,\tfrac{k_1}{2} & -k_2 & 0 & \tfrac{k_1}{2} + k_2 & \tfrac{k_1}{2} \\
-\,\tfrac{k_1}{2} & -\,\tfrac{k_1}{2} & 0 & 0 & \tfrac{k_1}{2} & \tfrac{k_1}{2}
\end{bmatrix}.
$$

#### Comment

This global stiffness matrix is identical to the matrix obtained earlier by writing nodal equilibrium directly (Section 3.2). The Direct Stiffness Method therefore provides a systematic element-by-element route to the same global system:

$$
[K]\{U\}=\{F\}.
$$

You may now apply boundary conditions (known displacements) to reduce the system and solve for the unknown nodal displacements, then back-substitute to compute element axial forces, strains and stresses.

---

### Streamlined Assembly Procedure in the Direct Stiffness Method  

The previous worked example illustrated the **direct stiffness method** by explicitly computing each entry of the global stiffness matrix. While conceptually clear, this approach becomes **cumbersome and inefficient** for large systems.  

A more efficient method, especially suited for **computer implementation**, proceeds as follows:  

1. Each element stiffness matrix is first transformed into the **global coordinate system**.  
2. The terms of this element matrix are then **added directly** to the appropriate entries of the global stiffness matrix using a **nodal connectivity table**.  
3. Once an element’s contributions are added, that element is no longer revisited.  

#### Element stiffness matrices with global DOF labels  

To illustrate, consider the two-element truss from **Figure 3.2** again. The element stiffness matrices in global form are written as:

For **Element 1** (connected at global DOFs 1, 2, 5, 6):  

$$
K^{(1)} =
\begin{bmatrix}
k^{(1)}_{11} & k^{(1)}_{12} & k^{(1)}_{13} & k^{(1)}_{14} \\
k^{(1)}_{21} & k^{(1)}_{22} & k^{(1)}_{23} & k^{(1)}_{24} \\
k^{(1)}_{31} & k^{(1)}_{32} & k^{(1)}_{33} & k^{(1)}_{34} \\
k^{(1)}_{41} & k^{(1)}_{42} & k^{(1)}_{43} & k^{(1)}_{44}
\end{bmatrix}
\quad
\text{with rows/cols mapped to } [1,2,5,6]
\tag{3.37}
$$

For **Element 2** (connected at global DOFs 3, 4, 5, 6):  

$$
K^{(2)} =
\begin{bmatrix}
k^{(2)}_{11} & k^{(2)}_{12} & k^{(2)}_{13} & k^{(2)}_{14} \\
k^{(2)}_{21} & k^{(2)}_{22} & k^{(2)}_{23} & k^{(2)}_{24} \\
k^{(2)}_{31} & k^{(2)}_{32} & k^{(2)}_{33} & k^{(2)}_{34} \\
k^{(2)}_{41} & k^{(2)}_{42} & k^{(2)}_{43} & k^{(2)}_{44}
\end{bmatrix}
\quad
\text{with rows/cols mapped to } [3,4,5,6]
\tag{3.38}
$$  

Here, the indices to the right of each row and above each column indicate the **global displacement number** associated with that row/column of the element stiffness matrix.  

#### Example of placement  

From Eq. (3.38), the entry \(k^{(2)}_{24}\) corresponds to global DOFs (row = 4, col = 6).  
Thus it contributes to the global entry:  

$$
K_{46} \quad \text{(and by symmetry, also } K_{64}\text{).}
$$  

This systematic allocation ensures that **all element contributions are assembled consistently** into the global matrix.

#### Element-node connectivity  

For computer implementation, the **element-node connectivity table** is used. It lists each element and the two global nodes it connects. For the two-element truss in Figure 3.2:  

**Table 3.2 – Element-Node Connectivity**  

| Element | Node \(i\) | Node \(j\) |
|---------|------------|------------|
| 1       | 1          | 3          |
| 2       | 2          | 3          |

From this, the **element displacement location vector** is defined as:  

$$
L^{(e)} = [2i-1,\; 2i,\; 2j-1,\; 2j]
\tag{3.39}
$$  

For the two elements:  

- \(L^{(1)} = [1, 2, 5, 6]\)  
- \(L^{(2)} = [3, 4, 5, 6]\)  

This vector indicates the **global DOFs** associated with the element stiffness matrix.  

#### Interpretation  

Think of the global stiffness matrix as a **6×6 grid of mailboxes**, initially empty (all zero).  
Each element stiffness matrix is like a bundle of values with **addresses** specified by the displacement location vector.  
When processing an element, each of its stiffness entries is dropped into the correct mailbox.  

Once all elements are processed, the mailbox grid contains the **assembled global stiffness matrix**, ready for applying boundary conditions and solving:  

$$
[K]\{U\} = \{F\}.
$$

---

### Boundary Conditions and Constraint Forces  

<img width="1053" height="566" alt="image" src="https://github.com/user-attachments/assets/4f408adc-8d87-4dc7-9e83-a3003bba3a42" />

Once the **global stiffness matrix** has been assembled, the governing system of equations for the example truss of Figure 3.2 can be expressed as:  

$$
[K]
\begin{Bmatrix}
U_1 \\ U_2 \\ U_3 \\ U_4 \\ U_5 \\ U_6
\end{Bmatrix}
=
\begin{Bmatrix}
F_1 \\ F_2 \\ F_3 \\ F_4 \\ F_5 \\ F_6
\end{Bmatrix}
\tag{3.42}
$$  

Here,  
- $[K]$ is the global stiffness matrix (singular before boundary conditions),  
- $\{U\}$ is the vector of global nodal displacements,  
- $\{F\}$ is the vector of applied nodal forces.  

#### Role of boundary conditions  

The stiffness matrix $[K]$ is **singular**, so Equation (3.42) does not have a unique solution unless boundary conditions are applied. These boundary conditions represent the **support constraints** that prevent rigid body motion.  

For the truss in Figure 3.2, the supports imply:  

$$
U_1 = U_2 = U_3 = U_4 = 0
\tag{3.43}
$$  

leaving only the displacements $U_5$ and $U_6$ as unknowns.  

#### Reduced system equations  

Substituting these constraints into Equation (3.42), the system reduces to:  

$$
\begin{aligned}
K_{15}U_5 + K_{16}U_6 &= F_1 \\
K_{25}U_5 + K_{26}U_6 &= F_2 \\
K_{35}U_5 + K_{36}U_6 &= F_3 \\
K_{45}U_5 + K_{46}U_6 &= F_4 \\
K_{55}U_5 + K_{56}U_6 &= F_5 \\
K_{56}U_5 + K_{66}U_6 &= F_6
\end{aligned}
\tag{3.44}
$$  

- The first four equations correspond to **reaction forces** at the constrained supports (nodes 1 and 2).  
- The last two equations form the **active system**, which can be solved directly for the unknown displacements $U_5, U_6$.  

#### General partitioned form  

For larger systems, it is convenient to partition the equations into constrained (c) and active (a) sets:  

$$
\begin{bmatrix}
K_{cc} & K_{ca} \\
K_{ac} & K_{aa}
\end{bmatrix}
\begin{Bmatrix}
U_c \\ U_a
\end{Bmatrix}
=
\begin{Bmatrix}
F_c \\ F_a
\end{Bmatrix}
\tag{3.45}
$$  

- $\{U_c\}$: constrained (known) displacements, usually supports  
- $\{U_a\}$: active (unknown) displacements  
- $\{F_c\}$: reaction forces at supports  
- $\{F_a\}$: applied external loads  

#### Solution procedure  

From the **active partition**:  

$$
[K_{ac}]\{U_c\} + [K_{aa}]\{U_a\} = \{F_a\}
$$  

which gives:  

$$
\{U_a\} = [K_{aa}]^{-1} \big( \{F_a\} - [K_{ac}]\{U_c\} \big)
\tag{3.46}
$$  

This step yields the **active displacements**.  

Then, the **reactions** are obtained from the constrained partition:  

$$
\{F_c\} = [K_{cc}]\{U_c\} + [K_{ca}]\{U_a\}
$$  

where symmetry ensures:  

$$
[K_{ca}] = [K_{ac}]^T
$$  

✅ **Summary:**  
- Boundary conditions eliminate rigid body modes.  
- Unknown displacements are solved from the reduced active system.  
- Reactions are recovered afterward using the constrained equations.  

This systematic procedure applies to **all FEM models**, not just trusses.  

---

### Element Strain and Stress  

The final computational step in finite element analysis of a truss structure is to utilize the global displacements obtained in the solution step to determine the strain and stress in each element of the truss.  

For an element connecting nodes *i* and *j*, the element nodal displacements in the element coordinate system are given by Equations (3.19) and (3.20):  

$$
u^{(e)}_1 = U^{(e)}_1 \cos\theta + U^{(e)}_2 \sin\theta
\tag{3.48a}
$$  

$$
u^{(e)}_2 = U^{(e)}_3 \cos\theta + U^{(e)}_4 \sin\theta
\tag{3.48b}
$$  

The element axial strain (utilizing Equation 2.29 and the discretization and interpolation functions of Equation 2.25) is:  

$$
\varepsilon^{(e)} = \frac{du^{(e)}(x)}{dx}
= \frac{d}{dx}\,[N_1(x)\;N_2(x)]
\begin{Bmatrix}
u^{(e)}_1 \\ u^{(e)}_2
\end{Bmatrix}
= \frac{-1}{L^{(e)}}
\begin{bmatrix}
1 & -1
\end{bmatrix}
\begin{Bmatrix}
u^{(e)}_1 \\ u^{(e)}_2
\end{Bmatrix}
= \frac{u^{(e)}_2 - u^{(e)}_1}{L^{(e)}}
\tag{3.49}
$$  

where $L^{(e)}$ is the element length.  

The element axial stress is obtained via Hooke’s law:  

$$
\sigma^{(e)} = E \, \varepsilon^{(e)}
\tag{3.50}
$$  

Note, however, that the global solution does not give the element axial displacement directly.  
The element displacements are obtained from the global displacements via Equations (3.48).  
Recalling Equations (3.21) and (3.22), the element strain in terms of global system displacements is:  

$$
\varepsilon^{(e)} = \frac{du^{(e)}(x)}{dx}
= \frac{d}{dx}[N_1(x)\;N_2(x)][R]
\begin{Bmatrix}
U^{(e)}_1 \\ U^{(e)}_2 \\ U^{(e)}_3 \\ U^{(e)}_4
\end{Bmatrix}
\tag{3.51}
$$  

where $[R]$ is the element transformation matrix defined by Equation (3.22).  

Thus, the element stresses for the bar element in terms of global displacements are:  

$$
\sigma^{(e)} = E\,\varepsilon^{(e)}
= E \frac{du^{(e)}(x)}{dx}
= E \frac{d}{dx}[N_1(x)\;N_2(x)][R]
\begin{Bmatrix}
U^{(e)}_1 \\ U^{(e)}_2 \\ U^{(e)}_3 \\ U^{(e)}_4
\end{Bmatrix}
\tag{3.52}
$$  

As the bar element is formulated here:  
- A **positive axial stress** indicates that the element is in **tension**,  
- A **negative value** indicates **compression**,  
per the usual structural convention.  

⚠️ Note that the stress calculation indicated in Equation (3.52) must be performed **on an element-by-element basis** after the global displacements are determined.  
If desired, the element forces can be obtained via Equation (3.23).  

---

### Example 3.2 – Two-Element Truss with External Loading  

**Problem Statement**  
The two-element truss in Figure 3.5 is subjected to external loading as shown. Using the same node and element numbering as in Figure 3.2, determine:  
- The displacement components of node 3,  
- The reaction force components at nodes 1 and 2,  
- The element displacements, stresses, and forces.  

<img width="569" height="552" alt="image" src="https://github.com/user-attachments/assets/f6c2920a-76a4-42c1-b960-d9f1c85b9fc7" />

The elements have modulus of elasticity:  

$$
E_1 = E_2 = 10 \times 10^6 \, \text{lb/in}^2, 
\quad 
A_1 = A_2 = 1.5 \, \text{in}^2
$$  

**Solution:**  

**Nodal coordinates:**  

$$
\theta_1 = \pi/4, 
\quad 
\theta_2 = 0
$$  

**Element lengths:**  

$$
L_1 = \sqrt{40^2 + 40^2} \approx 56.57 \, \text{in.}, 
\quad 
L_2 = 40 \, \text{in.}
$$  

**Characteristic stiffnesses:**  

$$
k_1 = \frac{A_1 E_1}{L_1} = 2.65 \times 10^5 \, \text{lb/in.}, 
\quad 
k_2 = \frac{A_2 E_2}{L_2} = 3.75 \times 10^5 \, \text{lb/in.}
$$  

**Global stiffness matrix:**  

$$
[K] = 10^5
\begin{bmatrix}
1.325 & 1.325 & 0 & 0 & -1.325 & -1.325 \\
1.325 & 1.325 & 0 & 0 & -1.325 & -1.325 \\
0 & 0 & 3.75 & 0 & -3.75 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 \\
-1.325 & -1.325 & -3.75 & 0 & 5.075 & 1.325 \\
-1.325 & -1.325 & 0 & 0 & 1.325 & 1.325
\end{bmatrix}
$$  

### Step 1: Apply Constraints  

$$
U_1 = U_2 = U_3 = U_4 = 0
$$  

Reduced system:  

$$
10^5
\begin{bmatrix}
5.075 & 1.325 \\
1.325 & 1.325
\end{bmatrix}
\begin{Bmatrix}
U_5 \\ U_6
\end{Bmatrix}
=
\begin{Bmatrix}
500 \\ 300
\end{Bmatrix}
$$  

### Step 2: Solve Displacements  

$$
U_5 = 5.333 \times 10^{-4} \, \text{in.}, 
\quad 
U_6 = 1.731 \times 10^{-3} \, \text{in.}
$$  

### Step 3: Reaction Forces  

$$
\{F_c\} =
\begin{Bmatrix}
-300 \\ -300 \\ -200 \\ 0
\end{Bmatrix} \, \text{lb}
$$  

### Step 4: Element 1 Results  

Element displacements:  

$$
\{u^{(1)}\} =
\begin{Bmatrix}
0 \\ 1.6 \times 10^{-3}
\end{Bmatrix} \, \text{in.}
$$  

Stress:  

$$
\sigma^{(1)} \approx 283 \, \text{lb/in}^2 \quad (\text{tension})
$$  

Element forces:  

$$
\{f^{(1)}\} =
\begin{Bmatrix}
-424 \\ 424
\end{Bmatrix} \, \text{lb}
$$  

### Step 5: Element 2 Results  

Element displacements:  

$$
\{u^{(2)}\} =
\begin{Bmatrix}
0 \\ 0.5333 \times 10^{-3}
\end{Bmatrix} \, \text{in.}
$$  

Stress:  

$$
\sigma^{(2)} \approx 133 \, \text{lb/in}^2 \quad (\text{tension})
$$  

Element forces:  

$$
\{f^{(2)}\} =
\begin{Bmatrix}
-200 \\ 200
\end{Bmatrix} \, \text{lb}
$$  

### ✅ Summary of Results  

| Quantity | Result |
|----------|--------|
| **Displacements** | $U_5 = 5.333 \times 10^{-4} \, \text{in.}, \; U_6 = 1.731 \times 10^{-3} \, \text{in.}$ |
| **Reaction Forces** | $F_1 = -300 \, \text{lb}, \; F_2 = -300 \, \text{lb}, \; F_3 = -200 \, \text{lb}, \; F_4 = 0$ |
| **Element 1 Displacements** | $u^{(1)} = [0, \, 1.6 \times 10^{-3}] \, \text{in.}$ |
| **Element 1 Stress** | $\sigma^{(1)} = 283 \, \text{lb/in}^2 \, (\text{tension})$ |
| **Element 1 Forces** | $f^{(1)} = [-424, \, 424] \, \text{lb}$ |
| **Element 2 Displacements** | $u^{(2)} = [0, \, 0.5333 \times 10^{-3}] \, \text{in.}$ |
| **Element 2 Stress** | $\sigma^{(2)} = 133 \, \text{lb/in}^2 \, (\text{tension})$ |
| **Element 2 Forces** | $f^{(2)} = [-200, \, 200] \, \text{lb}$ |


📌 Both elements are in **tension**, and the FEM results agree with the direct stress formula:  

$$
\sigma = \frac{F}{A}
$$  

---

## Comprehensive Example  

As a comprehensive example of two-dimensional truss analysis, the structure depicted in **Figure 3.6a** is analyzed to obtain displacements, reaction forces, strains, and stresses. While not all computational details are included, this example illustrates the sequence of required steps in a finite element analysis.  

<img width="1170" height="500" alt="image" src="https://github.com/user-attachments/assets/81e8216f-4835-4137-91b8-982045cde155" />

### Step 1: Define Geometry and Connectivity  

- Global coordinate system specified.  
- Node numbers and element connectivity shown in **Figure 3.6b**.  

### Step 2: Compute Element Stiffness Values  

$$
k^{(1)} = k^{(3)} = k^{(4)} = k^{(5)} = k^{(7)} = k^{(8)} 
= \frac{1.5 \times 10^7}{40} = 3.75 \times 10^5 \; \text{lb/in.}
$$  

$$
k^{(2)} = k^{(6)} 
= \frac{1.5 \times 10^7}{40 \sqrt{2}} 
= 2.65 \times 10^5 \; \text{lb/in.}
$$  

### Step 3: Transform Element Stiffness Matrices  

Using Equation (3.28) with orientations:  

$$
\theta_1 = \theta_3 = \theta_5 = \theta_7 = 0, \quad
\theta_4 = \theta_8 = \pi/2, \quad
\theta_2 = \pi/4, \quad
\theta_6 = 3\pi/4
$$  

For example:  

$$
K^{(1)} = 3.75 \times 10^5
\begin{bmatrix}
1 & 0 & -1 & 0 \\
0 & 0 & 0 & 0 \\
-1 & 0 & 1 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}, 
\quad
K^{(2)} = \frac{2.65 \times 10^5}{2}
\begin{bmatrix}
1 & 1 & -1 & -1 \\
1 & 1 & -1 & -1 \\
-1 & -1 & 1 & 1 \\
-1 & -1 & 1 & 1
\end{bmatrix}
$$  

(Similar forms hold for $K^{(3)}$ through $K^{(8)}$).  

### Step 4: Define Connectivity  

**(a) Correspondence Table (Table 3.3):**  
Maps element displacements to global degrees of freedom.  

**(b) Element-Node Connectivity (Table 3.4):**  
Each element is associated with nodes $i, j$.  

Example:  

$$
L^{(1)} = [1 \; 2 \; 5 \; 6], \quad
L^{(2)} = [1 \; 2 \; 7 \; 8], \quad
\ldots
$$  

### Step 5: Assemble Global Stiffness Matrix  

Using the connectivity data, stiffness contributions from all 8 elements are assembled into the global $12 \times 12$ stiffness matrix $[K]$.  

### Step 6: Apply Boundary Conditions  

- Nodes 1 and 2 are **fixed**.  

$$
U_1 = U_2 = U_3 = U_4 = 0
$$  

This reduces the system to **8 active displacement equations**.  

### Step 7: Solve for Active Displacements  

Solving numerically (via spreadsheet, MATLAB, or FEM software):  

$$
\begin{Bmatrix}
U_5 \\ U_6 \\ U_7 \\ U_8 \\ U_9 \\ U_{10} \\ U_{11} \\ U_{12}
\end{Bmatrix}
=
\begin{Bmatrix}
0.02133 \\ 0.04085 \\ -0.01600 \\ 0.04619 \\ 
0.04267 \\ 0.15014 \\ -0.00533 \\ 0.16614
\end{Bmatrix} \; \text{in.}
$$  

### Step 8: Compute Reaction Forces  

Substituting displacements into the constraint equations:  

$$
\begin{Bmatrix}
F_1 \\ F_2 \\ F_3 \\ F_4
\end{Bmatrix}
=
\begin{Bmatrix}
-12{,}000 \\ -4{,}000 \\ 6{,}000 \\ 0
\end{Bmatrix} \; \text{lb}
$$  

### Step 9: Compute Strain and Stress  

For example, **Element 2**:  

$$
u^{(2)}_1 = 0, 
\quad 
u^{(2)}_2 = -0.01600 + 0.04618 = 0.02134 \; \text{in.}
$$  

$$
\varepsilon^{(2)} = \frac{u^{(2)}_2 - u^{(2)}_1}{L^{(2)}} 
= 3.77 \times 10^{-4}, 
\quad
\sigma^{(2)} = E \, \varepsilon^{(2)} = 3771 \; \text{psi}
$$  

### ✅ Final Results (Table 3.5)  

| Element | Strain ($\varepsilon$) | Stress (psi) |
|---------|------------------------|--------------|
| 1 | $5.33 \times 10^{-4}$ | 5333 |
| 2 | $3.77 \times 10^{-4}$ | 3771 |
| 3 | $-4.0 \times 10^{-4}$ | -4000 |
| 4 | $1.33 \times 10^{-4}$ | 1333 |
| 5 | $5.33 \times 10^{-4}$ | 5333 |
| 6 | $-5.67 \times 10^{-4}$ | -5657 |
| 7 | $2.67 \times 10^{-4}$ | 2667 |
| 8 | $4.00 \times 10^{-4}$ | 4000 |

---


## 📚 Reference

- [*Fundamentals of Finite Element Analysis - (Unit 1 - Chapter 1 to 3)*](Resources/Unit1_Ch_1_to_3_FEM_Hutton.pdf) – Hutton David, McGraw-Hill 





