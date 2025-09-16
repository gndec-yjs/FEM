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

### Direct Stiffness Method

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

### Nodal Equilibrium Equations

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

### Direct Assembly of Global Stiffness Matrix

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

- $K_{11} = k^{(1)}_{11} + 0$  
- $K_{12} = k^{(1)}_{12} + 0$  
- $K_{13} = 0 + 0$  
- $K_{14} = 0 + 0$  
- $K_{15} = k^{(1)}_{13} + 0$  
- $K_{16} = k^{(1)}_{14} + 0$  

- $K_{22} = k^{(1)}_{22} + 0$  
- $K_{23} = 0 + 0$  
- $K_{24} = 0 + 0$  
- $K_{25} = k^{(1)}_{23} + 0$  
- $K_{26} = k^{(1)}_{24} + 0$  

- $K_{33} = 0 + k^{(2)}_{11}$  
- $K_{34} = 0 + k^{(2)}_{12}$  
- $K_{35} = 0 + k^{(2)}_{13}$  
- $K_{36} = 0 + k^{(2)}_{14}$  

- $K_{44} = 0 + k^{(2)}_{22}$  
- $K_{45} = 0 + k^{(2)}_{23}$  
- $K_{46} = 0 + k^{(2)}_{24}$  

- $K_{55} = k^{(1)}_{33} + k^{(2)}_{33}$  
- $K_{56} = k^{(1)}_{34} + k^{(2)}_{34}$  

- $K_{66} = k^{(1)}_{44} + k^{(2)}_{44}$  

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

#### Step 1: Transformation for Element 1

For $\theta_1 = \pi/4$:  

- $\cos\theta_1 = \sin\theta_1 = \tfrac{\sqrt{2}}{2}$  
- Therefore: $\cos^2\theta_1 = \sin^2\theta_1 = \cos\theta_1\sin\theta_1 = \tfrac{1}{2}$  

Substituting into Equation (3.28):  

$$
[K^{(1)}] = \frac{k_1}{2}
\begin{bmatrix}
1 & 1 & -1 & -1 \\
1 & 1 & -1 & -1 \\
-1 & -1 & 1 & 1 \\
-1 & -1 & 1 & 1
\end{bmatrix}
$$

#### Step 2: Transformation for Element 2

For $\theta_2 = 0$:  

- $\cos\theta_2 = 1$, $\sin\theta_2 = 0$  

Hence:

$$
[K^{(2)}] = k_2
\begin{bmatrix}
1 & 0 & -1 & 0 \\
0 & 0 & 0 & 0 \\
-1 & 0 & 1 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

#### Step 3: Assembly Using Connectivity

Using the connectivity relations (Eqs. 3.35–3.36), the element contributions are placed into the **6×6 global matrix**.  

The assembled terms are:  

- $K_{11} = k_1/2$, $K_{12} = k_1/2$, $K_{15} = -k_1/2$, $K_{16} = -k_1/2$  
- $K_{22} = k_1/2$, $K_{25} = -k_1/2$, $K_{26} = -k_1/2$  
- $K_{33} = k_2$, $K_{35} = -k_2$  
- $K_{55} = k_1/2 + k_2$, $K_{56} = k_1/2$  
- $K_{66} = k_1/2$  


#### Step 4: Final Global Stiffness Matrix

Thus, the **assembled global stiffness matrix** is:

$$
[K] =
\begin{bmatrix}
\;\; k_1/2 & \;\; k_1/2 & 0 & 0 & -k_1/2 & -k_1/2 \\
\;\; k_1/2 & \;\; k_1/2 & 0 & 0 & -k_1/2 & -k_1/2 \\
0 & 0 & k_2 & 0 & -k_2 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 \\
-\,k_1/2 & -\,k_1/2 & -k_2 & 0 & k_1/2 + k_2 & k_1/2 \\
-\,k_1/2 & -\,k_1/2 & 0 & 0 & k_1/2 & k_1/2
\end{bmatrix}
$$


**Result:**  
The global stiffness matrix obtained by the **Direct Stiffness Method** is identical to that obtained earlier by the **Nodal Equilibrium Equations** approach. This confirms the consistency of the two formulations.

---


