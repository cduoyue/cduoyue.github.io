---
layout: post
title: "Use MathJax to write LaTex expressions"
date: 2025-05-22
categories: necessary skills
author: Neoren
---

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [['$','$'], ['\\(','\\)']],
      displayMath: [['$$','$$'], ['\\[','\\]']],
      processEscapes: true,
      skipTags: ['script', 'noscript', 'style', 'textarea', 'pre']
    }
  });
</script>
<script async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"></script>



## Original code

[original code of the blog is here](https://github.com/cduoyue/cduoyue.github.io/blob/main/_posts/2025-05-22-Use-MathJax-to-write-LaTex-expressions.md?plain=1)



## Basic syntax

### Inline equations

Einstein mass-energy equation: $E=mc^2$

### Equation block

Einstein mass-energy equation:
$$
E=mc^2
$$



## Common math symbols

### Superscript/subscript

$x^2$

$x^{2y}$

$x_2$

$x_{2y}$

### Fraction

$\frac{x}{y}$

$\frac{x+1}{x-1}$

### Square root

$\sqrt{x}$

$\sqrt[3]{x}$

### Sum & Integration

$\sum_{i=1}^{n} x_i$

$\int_{0}^{1} f(x)dx$

### Greek letters

$\alpha, \beta, \gamma, \pi, \theta$

### Matrix

$$\begin{bmatrix} a & b \\ c & d \end{bmatrix}$$

$$\begin{pmatrix} a & b \\ c & d \end{pmatrix}$$





