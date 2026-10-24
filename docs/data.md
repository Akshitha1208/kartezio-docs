---
title: Benchmark and Data Protocol
description: Benchmark context and the proposed separation of optimization and evaluation conditions.
---

# Benchmark and Data Protocol

## Benchmark

The project uses the benchmark by Khan, Mikut, and Reischl, described in the literature survey. It was designed to study illumination robustness in object segmentation and classification. The survey reports nine object classes, ground-truth annotations, and controlled variations in shading and background noise.

The benchmark enables comparisons between performance on less-degraded images and performance under altered imaging conditions. Results should be interpreted within the benchmark's images and tested variations.

## Optimization and held-out conditions

To assess generalization, distinguish images or imaging conditions used to optimize candidate pipelines from conditions reserved for evaluation. The held-out set must not guide pipeline selection or parameter tuning. Where possible, hold out conditions (such as shading or noise levels), not just individual images with otherwise familiar conditions.

The final report should state how the data were divided, which conditions were used in each split, and whether any selection decisions were made after viewing held-out results. The exact split and noise/shading settings are experimental choices and should be documented once finalized.

## Ground truth

Use the benchmark's segmentation annotations as reference masks when calculating segmentation-quality metrics. Ensure every method is evaluated against the same annotations and on the same evaluation images.

## Data availability and reproducibility

Record the benchmark version or source, selected classes and conditions, preprocessing steps, and any exclusions. Do not infer robustness to conditions that are not represented in the evaluation data.
