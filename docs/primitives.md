---
title: Evaluation Plan
description: Segmentation-quality and robustness measures for comparing evolved and conventional pipelines.
---

# Evaluation Plan

The repository reports both conventional overlap scores and segmentation-only measures adapted from the benchmark paper.

## Standard overlap metrics

| Measure | Definition | Interpretation |
| --- | --- | --- |
| Dice similarity coefficient | 2 × shared foreground pixels ÷ (predicted foreground pixels + ground-truth foreground pixels) | Overlap between prediction and ground truth; higher is better. |
| Intersection over union (IoU) | Shared foreground pixels ÷ pixels marked foreground in either mask | Shared area relative to the combined foreground area; higher is better. |

The implementation treats nonzero pixels as foreground. For two empty masks, Dice and IoU are 1; if only one mask is empty, both scores are 0. Precision and recall can also distinguish false-positive from false-negative errors.

## Benchmark segmentation quality

`Q_seg` combines two fuzzy membership scores: one for agreement in the number of connected foreground objects and one for pixel overlap. Each component is in [0, 1], where higher indicates better agreement. The object-count component uses a sequential blob-matching rule; the pixel component is based on 1 − IoU. The source implementation and detailed formula decisions are documented in the [metrics module](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/projects/illumination_robustness/evaluation/metrics.py) and [benchmark protocol](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/docs/benchmark_protocol.md).

## Robustness

For a scene, `R_seg` is the mean `Q_seg` over the 13-image series with shading level `b = 1…13` and noise level `n = b + 1`. The implementation also reports series-mean Dice and IoU. Report the full condition-wise scores as well as the summary so that performance degradation is visible.

`R_seg` is segmentation-only. The original benchmark paper's full robustness value includes a classification score that this project does not implement, so the two values are not directly comparable.

## Fair comparison

The corrected Python baseline report records these full-grid means across 728 images:

| Baseline | Mean Dice | Mean IoU | Mean Q_seg | 13-image-series R_seg |
| --- | ---: | ---: | ---: | ---: |
| Otsu thresholding | 0.7651 | 0.6596 | 0.6019 | 0.5725 |
| Sobel edge detection | 0.1597 | 0.1380 | 0.1472 | 0.2084 |

These are baseline measurements, not results from evolved pipelines. The Python Sobel implementation approximates MATLAB's `edge(..., 'sobel')`; it is not numerically equivalent. See the repository's [corrected baseline reproduction report](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/docs/baseline_reproduction_report.md).

## Reproducibility and limitations

Compare methods on the same held-out images and conditions, with the same ground truth and metrics. Report results across independent evolutionary runs where feasible; a single best run can hide variability. Include the operation set, search settings, split, and random seeds. Runtime and pipeline complexity may provide additional context, but should be presented only if measured consistently.

The synthetic pilot is an integration check, not evidence of benchmark robustness. Do not treat high optimization fitness as evidence of generalization. Conclusions should reflect the tested benchmark and conditions.
