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

## Unit 4: Finite Element Types and Numerical Integration

**Types:** Triangular Elements, Rectangular Elements, Three-dimensional elements, Iso-parametric Formulation, Axis-Symmetric elements, Numerical integration, Gaussian quadrature

1. [Introduction](#41-introduction)
2. [Triangular Elements](#42-triangular-elements)
   - [Linear (CST) Triangular Element](#421-linear-cst-triangular-element)
   - [Quadratic (LST) Triangular Element](#422-quadratic-lst-triangular-element)
3. [Rectangular Elements](#43-rectangular-elements)
   - [Bilinear Quadrilateral Element (Q4)](#431-bilinear-quadrilateral-element-q4)
   - [Higher-Order Quadrilateral Elements](#432-higher-order-quadrilateral-elements)
4. [Three-Dimensional Elements](#44-three-dimensional-elements)
   - [Tetrahedral Element](#441-tetrahedral-element)
   - [Hexahedral (Brick) Element](#442-hexahedral-brick-element)
5. [Iso-parametric Formulation](#45-iso-parametric-formulation)
   - [Concept of Iso-parametric Elements](#451-concept-of-iso-parametric-elements)
   - [Jacobian Matrix and Coordinate Transformation](#452-jacobian-matrix-and-coordinate-transformation)
6. [Axis-Symmetric Elements](#46-axis-symmetric-elements)
7. [Numerical Integration](#47-numerical-integration)
   - [Need for Numerical Integration](#471-need-for-numerical-integration)
   - [Gaussian Quadrature (1D and 2D)](#472-gaussian-quadrature-1d-and-2d)
8. [Summary](#48-summary)

---

## 4.1 Introduction  

The efficiency and accuracy of the Finite Element Method (FEM) largely depend on the **type of element** used and the **integration scheme** applied during stiffness matrix computation.  
Finite elements are majorly classifies into several categories — **triangular**, **rectangular (quadrilateral)**, and **three-dimensional** — each having unique interpolation and geometric characteristics.  
This unit discusses these element types, the concept of **iso-parametric formulation**, and **numerical integration** (particularly Gaussian quadrature), which plays a key role in assembling global stiffness matrices.

---

## 4.2 Triangular Elements  

### 4.2.1 Linear (CST) Triangular Element  
*(Constant Strain Triangle – 3-noded element)*  

### 4.2.2 Quadratic (LST) Triangular Element  
*(Linear Strain Triangle – 6-noded element)*  

---

## 4.3 Rectangular Elements  

### 4.3.1 Bilinear Quadrilateral Element (Q4)  

### 4.3.2 Higher-Order Quadrilateral Elements  

---

## 4.4 Three-Dimensional Elements  

---

## 4.5 Iso-parametric Formulation  

### 4.5.1 Concept of Iso-parametric Elements  

### 4.5.2 Jacobian Matrix and Coordinate Transformation  

---

## 4.6 Axis-Symmetric Elements  

---

## 4.7 Numerical Integration  

### 4.7.1 Need for Numerical Integration  

### 4.7.2 Gaussian Quadrature (1D and 2D)  


---

## 4.8 Summary  


---
