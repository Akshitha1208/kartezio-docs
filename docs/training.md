---
title: Methodology
description: Study workflow for evolving and evaluating classical image-processing pipelines.
---

# Methodology

The project uses the established CGP approach implemented through Kartezio; the focus is its application and evaluation, not inventing a new evolutionary algorithm. Some workflow components are implemented and tested, while benchmark-scale pipeline evolution is still planned.

## 1. Define the operation set

Candidate graphs use Kartezio's matrix-operation library and project-specific primitives. The pilot library adds an Otsu threshold operation and border clearing. The repository also defines intensity transformations, thresholding, smoothing, edge processing, morphology, hole filling, and small-object cleanup as pipeline operations or postprocessing utilities. See the [primitive implementation](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/tree/main/projects/illumination_robustness/primitives).

## 2. Prepare and split data

The loader reads the 4-scene hardware-items benchmark and its scene-level masks. The split helper implements leave-one-scene-out validation and provides quality-only and robustness-aware training-condition subsets. Input processing includes percentile normalization; annotations are resized and binarized as described in the [benchmark and data protocol](data.md).

## 3. Evolve candidate pipelines

Each candidate is a CGP graph whose nodes apply image-processing operations. The implemented pilot fitness adapter returns `1 − Q_seg` per image, which Kartezio minimizes. The pilot uses an `EndpointThreshold` and a 16-node graph configuration in its documented sweep. Final fitness strategies for benchmark training remain under development; the pilot adapter should not be mistaken for a completed robustness-aware training objective.

The pilot workflow has been exercised on deterministic synthetic fixtures, including varied shapes, touching objects, holes, border objects, an illumination gradient, noise, empty masks, and constant images. It separates synthetic training fixtures from four unseen fixtures and saves pipeline descriptions, fitness history, predictions, and a summary. This validates software integration, not benchmark generalization.

## 4. Evaluate baselines and evolved pipelines

Python and MATLAB-compatible Otsu and Sobel baseline workflows are implemented. The Python full-grid baseline covers all 728 images; the MATLAB reference workflow is used for paper-oriented checks. Once benchmark-scale evolution is run, evaluate selected pipelines on held-out scenes without retuning and compare them with baselines on the same images and masks.

## 5. Report results

Report per-image, per-scene, and per-condition Dice, IoU, `Q_seg`, and robustness results. Also report object-count error, pipeline complexity, search configuration, and variability across independent seeds where measured. Do not compare the segmentation-only robustness score directly with the paper's full robustness score, which includes a classification term not implemented here.

## Current completion status

Benchmark validation, preprocessing and metrics, baseline reproduction, and synthetic pilot evolution are implemented. The audited leave-one-scene-out split definitions are available. Final benchmark-scale evolutionary runs, multiple-seed comparisons, and analysis of evolved pipeline robustness and failure cases remain planned. The [implementation page](implementation.md) links to commands and source modules.

## Workflow

```text
Benchmark images and masks
          |
          +--> Optimization conditions --> CGP pipeline search
          |                                  |
          |                                  v
          +--> Held-out conditions <-- Selected pipeline
                                             |
                                  Segmentation metrics
                                             |
                                Baseline comparison
```
