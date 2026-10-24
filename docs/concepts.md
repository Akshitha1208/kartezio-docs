---
title: Research Background
description: Why illumination robustness matters and how classical, learned, and evolutionary methods relate to the project.
---

# Research Background

## Classical segmentation

Classical image processing offers operations such as thresholding, filtering, edge detection, and morphology. These operations are transparent and can be combined into pipelines, but a fixed operation order and manually selected parameters may not transfer well between images.

Global thresholding is especially sensitive to spatially varying illumination because a single intensity cutoff may not separate foreground from background across an entire image. Adaptive thresholds and illumination normalization can address some lighting variation; filtering and morphology can reduce noise and refine masks. Each choice has trade-offs: smoothing may blur boundaries, edge operators may respond to lighting gradients, and morphology may alter small structures.

## Related approaches

Illumination-correction and low-light enhancement methods attempt to normalize image appearance before segmentation. Their visual improvement does not necessarily produce a more accurate mask, so they need to be assessed by downstream segmentation quality.

Learned segmentation and image-translation methods provide a contrasting approach. They can model complex patterns but typically require training data and more computation; performance on imaging conditions absent from training is not guaranteed.

## Why evolutionary pipeline generation?

Evolutionary computation turns the selection and arrangement of operations into a search problem. In this project, CGP represents a candidate pipeline as a graph: nodes represent image-processing operations and connections specify how intermediate results flow through the graph. Segmentation performance guides the search.

Because evolved pipelines use recognizable operations, their structure can be inspected. However, evolution alone does not guarantee robustness. Optimizing on favourable or narrow conditions can produce a pipeline that performs poorly elsewhere. This motivates testing on illumination and noise conditions that were not used during optimization.

## Research focus

The central question is whether CGP-generated classical pipelines can maintain useful segmentation quality across changing illumination and noise, rather than only performing well on optimization images. The project emphasizes controlled empirical evaluation and does not claim in advance that evolved pipelines outperform conventional or learned alternatives.
