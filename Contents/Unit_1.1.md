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

Once the global stiffness matrix has been assembled, the system equilibrium equations can be expressed in compact form as:  

$$
[K]\{U\} = \{F\}
\tag{3.42}
$$  

where:  
- \([K]\) is the global stiffness matrix (singular before applying constraints),  
- \(\{U\}\) is the vector of global nodal displacements,  
- \(\{F\}\) is the vector of applied nodal forces.  

#### Role of Boundary Conditions  

Because the global stiffness matrix includes rigid body motion, it is **singular** unless support constraints are imposed. These boundary conditions specify certain nodal displacements as known values (usually zero for supports).  

For the two-element truss of Figure 3.2, the support conditions enforce:  

$$
U_1 = U_2 = U_3 = U_4 = 0
\tag{3.43}
$$  

leaving only \(U_5\) and \(U_6\) as unknown displacements.  

#### Reduced System of Equations  

Substituting the boundary conditions into Eq. (3.42), the equations reduce to:  

\[
\begin{aligned}
K_{15}U_5 + K_{16}U_6 &= F_1 \\
K_{25}U_5 + K_{26}U_6 &= F_2 \\
K_{35}U_5 + K_{36}U_6 &= F_3 \\
K_{45}U_5 + K_{46}U_6 &= F_4 \\
K_{55}U_5 + K_{56}U_6 &= F_5 \\
K_{56}U_5 + K_{66}U_6 &= F_6
\end{aligned}
\tag{3.44}
\]

Here:  
- \(F_1, F_2, F_3, F_4\) = **reaction forces** at the constrained nodes (supports),  
- \(F_5, F_6\) = **applied external forces** at node 3.  

Thus, the last two equations can be solved for displacements \(U_5, U_6\).  
Substituting these values back into the first four equations provides the corresponding reaction forces.  

#### General Matrix Formulation  

In the general case, we partition the global system into **constrained** (\(c\)) and **active/unconstrained** (\(a\)) displacement sets:  

$$
\begin{bmatrix}
K_{cc} & K_{ca} \\
K_{ac} & K_{aa}
\end{bmatrix}
\begin{Bmatrix}
U_c \\
U_a
\end{Bmatrix}
=
\begin{Bmatrix}
F_c \\
F_a
\end{Bmatrix}
\tag{3.45}
$$  

where:  
- \(U_c\): known (prescribed) displacements,  
- \(U_a\): unknown displacements,  
- \(F_c\): unknown reaction forces,  
- \(F_a\): applied external forces.  

From the lower partition:  

\[
[K_{ac}]\{U_c\} + [K_{aa}]\{U_a\} = \{F_a\}
\]

so that:  

$$
\{U_a\} = [K_{aa}]^{-1}\left( \{F_a\} - [K_{ac}]\{U_c\} \right)
\tag{3.46}
$$  

Once the active displacements are determined, the **reaction forces** follow from the upper partition:  

$$
\{F_c\} = [K_{cc}]\{U_c\} + [K_{ca}]\{U_a\}
$$  

where the symmetry condition ensures:  

$$
[K_{ca}] = [K_{ac}]^T
$$  

---

## Element Strain and Stress  

The final step in the finite element analysis of a truss is to compute the **strain** and **stress** in each element using the global displacements obtained from the solution step.  

#### Element Nodal Displacements  

For an element connecting nodes \(i\) and \(j\), the displacements in the **element coordinate system** are:  

\[
\begin{aligned}
u^{(e)}_1 &= U^{(e)}_1 \cos\theta + U^{(e)}_2 \sin\theta \\
u^{(e)}_2 &= U^{(e)}_3 \cos\theta + U^{(e)}_4 \sin\theta
\end{aligned}
\tag{3.48}
\]

Here:  
- \(u^{(e)}_1, u^{(e)}_2\) = element nodal displacements along the element axis,  
- \(U^{(e)}_1, U^{(e)}_2, U^{(e)}_3, U^{(e)}_4\) = global nodal displacements,  
- \(\theta\) = element orientation angle.  

#### Element Strain  

Using the displacement interpolation functions, the **axial strain** in the element is:  

\[
\varepsilon^{(e)} = \frac{du^{(e)}(x)}{dx} = \frac{u^{(e)}_2 - u^{(e)}_1}{L^{(e)}}
\tag{3.49}
\]

where \(L^{(e)}\) is the length of the element.  

#### Element Stress  

Applying Hooke’s Law, the **axial stress** in the element is:  

\[
\sigma^{(e)} = E \, \varepsilon^{(e)}
\tag{3.50}
\]

with \(E\) being the modulus of elasticity of the element material.  

#### Strain and Stress in Global Displacement Form  

The global finite element solution provides **global displacements**, not element displacements directly. To connect the two, the transformation relations (Equations 3.21–3.22) are used. In terms of global nodal displacements, the strain becomes:  

\[
\varepsilon^{(e)} = \frac{d}{dx}[N_1(x) \; N_2(x)] [R] 
\begin{Bmatrix}
U^{(e)}_1 \\ U^{(e)}_2 \\ U^{(e)}_3 \\ U^{(e)}_4
\end{Bmatrix}
\tag{3.51}
\]

and the corresponding stress is:  

\[
\sigma^{(e)} = E \, \varepsilon^{(e)} = E \frac{d}{dx}[N_1(x) \; N_2(x)] [R] 
\begin{Bmatrix}
U^{(e)}_1 \\ U^{(e)}_2 \\ U^{(e)}_3 \\ U^{(e)}_4
\end{Bmatrix}
\tag{3.52}
\]

Here:  
- \([R]\) = transformation matrix,  
- \(N_1(x), N_2(x)\) = shape functions,  
- Positive \(\sigma^{(e)}\) → element is in **tension**,  
- Negative \(\sigma^{(e)}\) → element is in **compression**.  

#### Element Forces  

Finally, the **element axial force** can also be obtained as:  

\[
\{f^{(e)}\} =
\begin{bmatrix}
-1 & 1 \\
\end{bmatrix}
\frac{AE}{L^{(e)}}
\begin{Bmatrix}
u^{(e)}_1 \\ u^{(e)}_2
\end{Bmatrix}
\tag{3.23 revisited}
\]

This provides the internal force carried by each truss member, consistent with the sign convention for stress.  

---

✅ With this step, the finite element procedure for a truss structure is complete:  
1. Assemble the global stiffness matrix,  
2. Apply boundary conditions and solve for global displacements,  
3. Back-substitute displacements to compute element strains, stresses, and internal forces.

---


