
# 📘 **MST-102: Finite Element Method in Structural Engineering**

### ✏️ Unit 1: Introduction


## 1.1 History and Applications of Finite Element Method

The **Finite Element Method (FEM)** originated in the 1950s for aerospace structural analysis. It gained traction in civil, mechanical, and automotive engineering during the 1960s–70s.

### 📌 Key Milestones

* 1943: Courant applied piecewise polynomial interpolation
* 1956: Turner et al. formalized FEM for structural frames and aircraft wings
* 1960s–80s: Extension to heat transfer, fluid mechanics, soil mechanics
* Modern usage: ANSYS, ABAQUS, ATENA, NASTRAN, SAP2000

### 🔧 Applications

* Analysis of beams, frames, trusses
* Stress distribution in machine parts
* Soil-structure interaction
* Earthquake-resistant design
* Thermal analysis
* Fatigue and fracture mechanics

---

## 1.2 Spring and Bar Elements

### 📍 Element Definition

A **spring element** is the simplest 1D structural element obeying Hooke’s Law:

$$
F = k \cdot u
$$

Where:

* $F$ = Force
* $k$ = Spring constant
* $u$ = Displacement

A **bar element** (or axial truss element) has 2 DOFs (one at each node). The stiffness matrix is derived using:

$$
k = \frac{AE}{L}
$$

Where:

* $A$ = Area of cross-section
* $E$ = Young’s modulus
* $L$ = Length

### 🔹 Stiffness Matrix (Local Coordinates)

For a 2-node axial bar element:

$$
\mathbf{k}^e = \frac{AE}{L}
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

### 🧠 **Example 1:**

Calculate the stiffness matrix for a steel bar of length 1 m, cross-section 100 mm², $E = 200 \times 10^9$ Pa.

**Solution:**

$$
k = \frac{100 \times 10^{-6} \times 200 \times 10^9}{1} = 20 \times 10^6 \, \text{N/m}
$$

So,

$$
\mathbf{k} = 20 \times 10^6
\begin{bmatrix}
1 & -1 \\
-1 & 1
\end{bmatrix}
$$

---

## 1.3 Minimum Potential Energy Principle

This principle states that **a system in equilibrium minimizes its total potential energy**:

$$
\Pi = U - W
$$

Where:

* $\Pi$ = Total potential energy
* $U$ = Strain energy
* $W$ = Work done by external forces

### ➕ Application:

Used to derive finite element equations via variational formulation.

---

## 1.4 Direct Stiffness Method

This method forms the basis of FEM by systematically assembling stiffness matrices.

### 📋 Steps:

1. Divide structure into elements
2. Derive local stiffness matrix
3. Assemble global stiffness matrix
4. Apply boundary conditions
5. Solve for nodal displacements

---

## 1.5 Nodal Equilibrium Equations

These are derived using force balance at each node:

$$
\mathbf{K} \cdot \mathbf{u} = \mathbf{f}
$$

Where:

* $\mathbf{K}$ = Global stiffness matrix
* $\mathbf{u}$ = Displacement vector
* $\mathbf{f}$ = Load vector

---

## 1.6 Assembly of Global Stiffness Matrix

Each element’s local stiffness matrix is expanded and added to a global matrix using **connectivity information**.

$$
\mathbf{K}_{global} = \sum_{e=1}^{n} \mathbf{k}^{e}
$$

### 🧠 **Example 2:**

Assemble the global stiffness matrix for a 2-element bar system (each with $AE/L = k$).

**Answer:**

Assume node connectivity:

* Element 1: Node 1–2
* Element 2: Node 2–3

$$
\mathbf{K}_{global} =
\begin{bmatrix}
k & -k & 0 \\
-k & 2k & -k \\
0 & -k & k
\end{bmatrix}
$$

---

## 1.7 Element Strain and Stress

### 📎 Strain–Displacement Relation:

$$
\varepsilon = \frac{du}{dx}
$$

For bar elements:

$$
\varepsilon^e = \frac{u_2 - u_1}{L}
$$

### 📎 Stress–Strain Relation:

$$
\sigma = E \cdot \varepsilon
$$

### ➕ Element Force:

$$
F = AE \cdot \varepsilon = \frac{AE}{L} (u_2 - u_1)
$$


## 📘 References:

* Seshu, P. *Finite Element Analysis*, PHI
* Cook, R.D. *Concepts and Applications of Finite Element Analysis*, Wiley
* Zienkiewicz, O.C. *Finite Element Method*, Elsevier
* Hutton, D. *Fundamentals of Finite Element Analysis*, McGraw-Hill

Let me know, and I’ll proceed.
