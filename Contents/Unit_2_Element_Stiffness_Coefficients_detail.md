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

## Element Stiffness Coefficients

### Shape Functions

The displacement function for a flexure element is:

$$
v(x) = N_1(x)v_1 + N_2(x)\theta_1 + N_3(x)v_2 + N_4(x)\theta_2 \tag{4.27a}
$$

The shape functions in terms of the dimensionless coordinate \(\xi = \frac{x}{L}\) are:

$$
\begin{aligned}
N_1(\xi) &= 1 - 3\xi^2 + 2\xi^3, \\
N_2(\xi) &= L(\xi - 2\xi^2 + \xi^3), \\
N_3(\xi) &= 3\xi^2 - 2\xi^3, \\
N_4(\xi) &= L(-\xi^2 + \xi^3)
\end{aligned} \tag{4.28}
$$

### Second Derivatives

Second derivatives with respect to \(x\) are required:

$$
\frac{d^2}{dx^2} = \frac{1}{L^2} \frac{d^2}{d\xi^2} \tag{4.46, 4.47}
$$

Compute the second derivatives with respect to \(\xi\):

$$
\begin{aligned}
\frac{d^2 N_1}{d\xi^2} &= -6 + 12\xi, \\
\frac{d^2 N_2}{d\xi^2} &= L(-4 + 6\xi), \\
\frac{d^2 N_3}{d\xi^2} &= 6 - 12\xi, \\
\frac{d^2 N_4}{d\xi^2} &= L(-2 + 6\xi)
\end{aligned} \tag{4.49}
$$

Then with respect to \(x\):

$$
\frac{d^2 N_i}{dx^2} = \frac{1}{L^2} \frac{d^2 N_i}{d\xi^2} \tag{4.50}
$$

### Stiffness Coefficients

The element stiffness coefficients are:

$$
k_{mn} = k_{nm} = E I_z \int_0^L \frac{d^2 N_m}{dx^2} \frac{d^2 N_n}{dx^2} dx \tag{4.51}
$$

Substitute dimensionless derivatives and \(dx = L d\xi\):

$$
k_{mn} = E I_z \int_0^1 \frac{d^2 N_m}{dx^2} \frac{d^2 N_n}{dx^2} (L d\xi)
= \frac{E I_z}{L^3} \int_0^1 \frac{d^2 N_m}{d\xi^2} \frac{d^2 N_n}{d\xi^2} d\xi \tag{4.52}
$$

### Step-by-Step Computation of Each Coefficient

**1. \(k_{11}\):**

$$
\begin{aligned}
k_{11} &= \frac{E I_z}{L^3} \int_0^1 (-6 + 12\xi)^2 d\xi \\
&= \frac{E I_z}{L^3} \int_0^1 (36 - 144\xi + 144\xi^2) d\xi \\
&= \frac{E I_z}{L^3} [36 - 72 + 48] \\
&= 12 \frac{E I_z}{L^3} \tag{4.53a}
\end{aligned}
$$

**2. \(k_{12} = k_{21}\):**

$$
\begin{aligned}
k_{12} &= \frac{E I_z}{L^3} \int_0^1 (-6 + 12\xi)(-4 + 6\xi) L d\xi \\
&= \frac{E I_z}{L^2} \int_0^1 (24 - 48\xi + 72\xi^2) d\xi \\
&= \frac{E I_z}{L^2} [24 - 24 + 12] \\
&= 6 \frac{E I_z}{L^2} \tag{4.53b}
\end{aligned}
$$

**3. \(k_{13} = k_{31}\):**

$$
\begin{aligned}
k_{13} &= \frac{E I_z}{L^3} \int_0^1 (-6 + 12\xi)(6 - 12\xi) d\xi \\
&= \frac{E I_z}{L^3} \int_0^1 (-36 + 144\xi - 144\xi^2) d\xi \\
&= \frac{E I_z}{L^3} [-36 + 72 - 24] \\
&= -12 \frac{E I_z}{L^3} \tag{4.53c}
\end{aligned}
$$

**4. \(k_{14} = k_{41}\):**

$$
\begin{aligned}
k_{14} &= \frac{E I_z}{L^3} \int_0^1 (-6 + 12\xi)(-2 + 6\xi) L d\xi \\
&= \frac{E I_z}{L^2} \int_0^1 (12 - 36\xi + 72\xi^2) d\xi \\
&= \frac{E I_z}{L^2} [12 - 18 + 12] \\
&= 6 \frac{E I_z}{L^2} \tag{4.53d}
\end{aligned}
$$

**5. \(k_{22}\):**

$$
k_{22} = \frac{E I_z}{L^3} \int_0^1 (-4 + 6\xi)^2 L^2 d\xi
= \frac{E I_z}{L} \int_0^1 (16 - 48\xi + 36\xi^2) d\xi
= 4 \frac{E I_z}{L} \tag{4.53e}
$$

**6. \(k_{23} = k_{32}\):**

$$
k_{23} = \frac{E I_z}{L^3} \int_0^1 (-4 + 6\xi)(6 - 12\xi) L d\xi
= -6 \frac{E I_z}{L^2} \tag{4.53f}
$$

**7. \(k_{24} = k_{42}\):**

$$
k_{24} = \frac{E I_z}{L^3} \int_0^1 (-4 + 6\xi)(-2 + 6\xi) L^2 d\xi
= 2 \frac{E I_z}{L} \tag{4.53g}
$$

**8. \(k_{33}\):**

$$
k_{33} = \frac{E I_z}{L^3} \int_0^1 (6 - 12\xi)^2 d\xi
= 12 \frac{E I_z}{L^3} \tag{4.53h}
$$

**9. \(k_{34} = k_{43}\):**

$$
k_{34} = \frac{E I_z}{L^3} \int_0^1 (6 - 12\xi)(-2 + 6\xi) L d\xi
= -6 \frac{E I_z}{L^2} \tag{4.53i}
$$

**10. \(k_{44}\):**

$$
k_{44} = \frac{E I_z}{L^3} \int_0^1 (-2 + 6\xi)^2 L^2 d\xi
= 4 \frac{E I_z}{L} \tag{4.53j}
$$

### Symmetry of Stiffness Matrix

The stiffness matrix is symmetric:

$$
k_{mn} = k_{nm} \tag{4.54}
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
