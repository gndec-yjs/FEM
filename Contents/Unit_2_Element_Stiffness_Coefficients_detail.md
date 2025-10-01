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

where the shape functions in terms of dimensionless coordinate $\xi = \frac{x}{L}$ are:

$$
\begin{aligned}
N_1(\xi) &= 1 - 3\xi^2 + 2\xi^3 \\
N_2(\xi) &= L(\xi - 2\xi^2 + \xi^3) \\
N_3(\xi) &= 3\xi^2 - 2\xi^3 \\
N_4(\xi) &= L(-\xi^2 + \xi^3) 
\end{aligned} \tag{4.28}
$$

### Second Derivatives

The second derivatives with respect to $x$ are required for the stiffness matrix:

$$
\frac{d^2}{dx^2} = \frac{1}{L^2} \frac{d^2}{d\xi^2} \tag{4.46, 4.47}
$$

Compute the second derivatives of $N_i(\xi)$ with respect to $\xi$:

$$
\begin{aligned}
\frac{d^2 N_1}{d\xi^2} &= -6 + 12\xi, \quad
\frac{d^2 N_2}{d\xi^2} = L(-4 + 6\xi), \\
\frac{d^2 N_3}{d\xi^2} &= 6 - 12\xi, \quad
\frac{d^2 N_4}{d\xi^2} = L(-2 + 6\xi)
\end{aligned} \tag{4.49}
$$

Then the second derivatives with respect to $x$:

$$
\frac{d^2 N_i}{dx^2} = \frac{1}{L^2} \frac{d^2 N_i}{d\xi^2} \tag{4.50}
$$

### Stiffness Coefficients

The element stiffness coefficients are defined as:

$$
k_{mn} = k_{nm} = E I_z \int_0^L \frac{d^2 N_m}{dx^2} \frac{d^2 N_n}{dx^2} dx \tag{4.51}
$$

Substitute the dimensionless derivatives and $dx = L\,d\xi$:

$$
k_{mn} = E I_z \int_0^1 \frac{d^2 N_m}{dx^2} \frac{d^2 N_n}{dx^2} (L \, d\xi)
= \frac{E I_z}{L^3} \int_0^1 \frac{d^2 N_m}{d\xi^2} \frac{d^2 N_n}{d\xi^2} d\xi \tag{4.52}
$$

### Computed Stiffness Coefficients

Performing the integration yields:

$$
\begin{aligned}
k_{11} &= 12 \frac{E I_z}{L^3}, & k_{12} &= 6 \frac{E I_z}{L^2}, & k_{13} &= -12 \frac{E I_z}{L^3}, & k_{14} &= 6 \frac{E I_z}{L^2}, \\
k_{22} &= 4 \frac{E I_z}{L}, & k_{23} &= -6 \frac{E I_z}{L^2}, & k_{24} &= 2 \frac{E I_z}{L}, \\
k_{33} &= 12 \frac{E I_z}{L^3}, & k_{34} &= -6 \frac{E I_z}{L^2}, & k_{44} &= 4 \frac{E I_z}{L}
\end{aligned} \tag{4.53}
$$

The matrix is symmetric:

$$
k_{mn} = k_{nm} \tag{4.54}
$$
