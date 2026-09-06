---
title: 'decoint: An IEEE 1788.1-2017 Compliant Interval Arithmetic Library for Python Using gmpy2'
tags:
  - Python
  - interval arithmetic
  - validated numerics
  - IEEE 1788
  - gmpy2
authors:
  - name: Arjav Sharma
    orcid: 0009-0001-6347-732X
    affiliation: 1
affiliations:
  - name: Independent Researcher
    index: 1
date: September 2026
bibliography: paper.bib
---

# Summary
`decoint` is an open-source Python library that implements rigorous, standard-compliant interval arithmetic according to the IEEE 1788.1-2017 standard for Simplified Interval Arithmetic. Developed to address the inherent unreliability of standard binary floating-point representations in critical applications, the library provides a robust framework for executing mathematical operations while explicitly bounding rounding and truncation errors. 

To guarantee strict containment bounds, `decoint` utilizes the arbitrary-precision capabilities of the `gmpy2` library, dynamically manipulating its underlying GNU MPFR rounding contexts during execution. A core strength of `decoint` is its comprehensive implementation of the IEEE 1788 decoration system—supporting `com` (common), `dac` (defined and continuous), `def` (defined), `trv` (trivial), and `ill` (ill-conditioned) states. This system tracks the mathematical validity and continuity of functions across intervals, preventing silent information loss during complex, multi-stage function evaluations. Ultimately, `decoint` offers an intuitive, pure-Python interface that brings low-level numerical rigor and error-bounding guarantees directly to the modern scientific Python ecosystem. Around 5000 test cases from the official ITF1788 repository are used to validate the core functions of the library.

# Statement of Need

In scientific computing, standard IEEE 754 binary floating-point arithmetic introduces deterministic limitations, including rounding errors, truncation, and catastrophic cancellation. While these discrepancies are microscopic in isolated calculations, they accumulate non-linearly over complex iterative algorithms, long-horizon numerical simulations, or high-dimensional matrix operations. This phenomenon of computational "numerical drift" can severely compromise the integrity of scientific models, leading to undetected structural failures or completely invalid outcomes in critical domains like global optimization, autonomous robotics path-planning, and verified physical simulations.

Without a mechanism to rigorously quantify error propagation, researchers risk relying on simulations that lack strict mathematical verification. Interval arithmetic addresses this vulnerability by replacing scalar values with closed intervals and executing operations over sets. However, basic interval implementations often restrict their evaluation entirely to the numerical infimum and supremum (lower and upper bounds), neglecting to monitor the underlying domain boundaries and mathematical continuity of functions over non-continuous execution spaces.

`decoint` fills this critical gap within the Python ecosystem. By wrapping `gmpy2` and executing hardware-enforced directed rounding—forcing rounding toward negative infinity for lower bounds and positive infinity for upper bounds—the library guarantees that the true mathematical result never escapes the computed containment bounds. Crucially, `decoint` integrates the full IEEE 1788 decoration subsystem. By explicitly tracking states such as continuity (`dac`) and defined domain compliance (`def`) during multi-stage function evaluations, it prevents silent informational degradation. This gives researchers an automated, standard-compliant tool to detect mathematical anomalies dynamically, providing provable numerical rigor directly within Python workflows.

# State of the Field

The ecosystem for interval computation varies significantly across modern programming environments. In ecosystems like Julia, rigorous numerical verification is highly advanced due to packages like `IntervalArithmetic.jl`, which offer native, highly optimized implementations of set-based interval calculations. Conversely, the Python scientific computing ecosystem lacks a unified, standard-compliant library that matches these capabilities.

Existing Python frameworks generally fall short of modern standards. Legacy packages like `pyinterval` provide basic interval representations but are no longer actively maintained and lack structural alignment with the unified specifications outlined in the modern IEEE 1788 framework. More recent libraries, such as `IntvalPy`, focus heavily on specialized interval linear systems, visualization, and classical or Kaucher interval arithmetic; however, they do not implement the complete IEEE 1788.1-2017 decoration subsystem. Without automated, exception-free decoration propagation (`com`, `dac`, `def`, `trv`, `ill`), these tools cannot dynamically monitor mathematical continuity or domain validity across multi-stage function evaluations. This leaves Python researchers without an accessible, out-of-the-box option that delivers both hardware-enforced numeric containment and rigorous execution-state tracking. `decoint` explicitly fills this gap, providing a streamlined, standard-aligned interface designed to meet the demands of modern computational science.

# Software Architecture and Implementation

`decoint` employs a decoupled, multi-layered object architecture designed to cleanly separate rigorous numeric containment from execution-state metadata tracking. The codebase exposes two primary user-facing classes: `Interval`, which handles the mathematical infimum and supremum bounds, and `DecoratedInterval`, a composite class that wraps an underlying `Interval` instance alongside a corresponding IEEE 1788 decoration object.

The underlying computational pipeline operates through a strict three-tier hierarchy:

1. **The Rounding Layer:** At the lowest level, the library abstracts directed rounding through specialized, decoupled primitive operations (e.g., `sin_up` and `sin_down`). Each primitive explicitly configures a local, isolated `gmpy2` context, modifying the underlying GNU MPFR rounding flags to guarantee directed truncation towards positive or negative infinity respectively, without altering the user's global runtime environment.
2. **The Interval Function Wrappers:** Built above the primitive rounding layer, these wrappers handle algorithmic transformations across non-monotonic function domains. When evaluating an interval, the wrapper isolates the critical points, boundaries, and global extrema (maximum and minimum bounds) over the target subset, resolving numerical edge cases securely before returning a raw bounded `Interval`.
3. **The DecoratedInterval Function Wrappers:** Operating at the highest tier, this wrapper intercepts the evaluated arithmetic to analyze the function's structural characteristics over the input domain. By dynamically parsing the tracking history for domain violations, mathematical discontinuities, poles, or infinite boundaries, this layer determines and appends the correct standard decoration status post-calculation.

# Research Impact Statement

`decoint` provides immediate utility to numerical analysis and validation workflows by enabling reproducible, error-bounded calculations directly within standard Python research scripts. By exposing clear community-readiness signals, including comprehensive unit testing against IEEE-defined edge cases and structural execution tracking, the package offers a baseline tool for verifying the stability of loss functions, chaotic differential equations, and global minimization routines in academic environments.

# AI Usage Disclosure

Generative artificial intelligence tools were utilized during the development of this project for automated code review, debugging assistance, and manuscript copy editing. All core code implementations, mathematical logic, and final manuscript content were comprehensively reviewed, verified, and finalized by the human author to ensure complete accuracy and adherence to standards.

# Acknowledgements

The author acknowledges the IEEE 1788 working group for establishing the foundational software and specifications that enabled this project.

# References
