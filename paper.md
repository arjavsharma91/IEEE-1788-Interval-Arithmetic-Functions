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
`decoint` is a Python library that implements interval arithmetic according to the IEEE 1788.1-2017 standard for Simplified Interval Arithmetic. It was developed to address floating point rounding errors in critical applications. The library provides a framework for executing standard set based interval arithmetic while guaranteeing full containment using directed rounding. To ensure strict containment, `decoint` uses gmpy2 MPFR values to enforce rounding modes. Around 5000 test cases that are pulled from the official ITF1788 repository are used to ensure that the library is fully standards compliant.


The library uses the IEEE 1788 decoration system, containing 5 decorations, to provide metadata to calculations. This system provides useful metadata during calculations to ensure the validity and provide context to the provided result. Essentially, `decoint` provides rigorous error bounding and low level numerical safety to a pure pythonic interface.

# Statement of Need

In scientific computing, IEEE 754 binary64 floating point arithmetic introduces reliability limitations, primarily rounding errors due to the inability to represent fractions whose denominators are not a power of 2. Often, these errors are irrelevant in minor calculations, however they tend to accumulate exponentially over iterative algorithms and matrix operations. This numerical drift can compromise the reliability of scientific computations and safety critical systems.

Researchers are often forced to use arithmetic while suffering through floating point error propagation throughout calculations. Interval Arithmetic solves this problem by replacing scalar values with set based calculations. However, basic interval implementations often restrict their evaluation entirely to the lower and upper bounds, neglecting to monitor the underlying domain boundaries and continuity of functions over non-continuous execution spaces.

`decoint` fills this gap within the Python scientific computing community. By wrapping `gmpy2` and executing hardware enforced directed rounding the library ensures that the true results never escape the containment bounds computed. `decoint` also integrates the full IEEE 1788 decoration subsystem. The library prevents silent informational degradation by tracking domain bounds and continuity to track the validity of a result. This gives researchers astandard compliant tool to detect mathematical anomalies, providing numerical rigor directly within Python.

# State of the Field

The ecosystem for interval computation varies significantly across modern programming environments. In ecosystems like Julia, rigorous numerical verification is highly advanced due to packages like `IntervalArithmetic.jl`, which offer native, highly optimized implementations of set-based interval calculations. Conversely, the Python scientific computing ecosystem lacks a unified, standard-compliant library that matches these capabilities.

Existing Python frameworks generally fall short of modern standards. Legacy packages like `pyinterval` provide basic interval representations but are no longer actively maintained and lack structural alignment with the unified specifications outlined in the modern IEEE 1788 framework. More recent libraries, such as `IntvalPy`, focus heavily on specialized interval linear systems, visualization, and classical or Kaucher interval arithmetic; however, they do not implement the complete IEEE 1788.1-2017 decoration subsystem. Without automated, exception-free decoration propagation (`com`, `dac`, `def`, `trv`, `ill`), these tools cannot dynamically monitor mathematical continuity or domain validity across multi-stage function evaluations. This leaves Python researchers without an accessible, out-of-the-box option that delivers both hardware-enforced numeric containment and rigorous execution-state tracking. `decoint` explicitly fills this gap, providing a streamlined, standard-aligned interface designed to meet the demands of modern computational science.

# Software Architecture and Implementation

`decoint` employs a decoupled, multi-layered object architecture designed to cleanly separate rigorous numeric containment from execution-state metadata tracking. The codebase exposes two primary user-facing classes: `Interval`, which handles the mathematical infimum and supremum bounds, and `DecoratedInterval`, a composite class that wraps an underlying `Interval` instance alongside a corresponding IEEE 1788 decoration object. The library uses standard 64 bit precision.

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
