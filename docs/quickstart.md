---
title: Scope and Research Questions
description: Project boundaries and the questions guiding the experimental study.
---

# Scope and Research Questions

## Research questions

1. Can CGP generate useful image-segmentation pipelines from classical image-processing operations?
2. How does segmentation quality change as illumination and background noise vary?
3. Do pipelines retain performance on conditions that were not represented during optimization?
4. How do evolved pipelines compare with conventional image-processing approaches under the same evaluation conditions?

## Scope

The project covers:

- Classical image-processing operations, including filtering, thresholding, edge detection, and morphology.
- CGP as the method for representing and evolving pipeline structures, using the Kartezio framework.
- Image segmentation evaluated with ground-truth annotations.
- Empirical comparisons under variations in illumination and noise.

The project does not aim to develop a new evolutionary algorithm or to present a learned segmentation model as its primary contribution. Learned approaches provide related-work context only.

## Study boundaries

The conclusions will be tied to the benchmark, operations, optimization conditions, and evaluation protocol used. Strong results on optimization images alone are not evidence of generalization; held-out conditions are needed to assess transfer beyond the search data.
