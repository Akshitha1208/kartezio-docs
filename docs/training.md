---
title: Methodology
description: Study workflow for evolving and evaluating classical image-processing pipelines.
---

# Methodology

The study follows an empirical workflow. The project uses the established CGP approach implemented through Kartezio; the focus is its application and evaluation, not inventing a new evolutionary algorithm.

## 1. Define the operation set

Select the classical image-processing operations available to pipeline candidates, such as filtering, thresholding, edge detection, and morphology. Document operation parameters and any constraints on candidate pipeline structure so that the search space is clear.

## 2. Prepare the benchmark data

Prepare input images and their ground-truth segmentation masks. Separate the optimization data from held-out images or imaging conditions before running evolution. See the [benchmark and data protocol](data.md).

## 3. Evolve candidate pipelines

Represent each candidate as a CGP graph whose nodes apply image-processing operations. Evaluate candidates against the reference masks on optimization data and use segmentation performance to guide evolutionary selection and variation.

Record the search configuration, operation set, fitness definition, stopping criteria, and random seeds. These settings should be reported with results; they are not prescribed in this documentation because the final experimental configuration has not been supplied.

## 4. Evaluate without retuning

Apply selected pipelines to held-out data without using those results to adjust the candidates. Evaluate conventional image-processing baselines on the same data and under the same conditions.

## 5. Report and compare

Report segmentation metrics by condition, changes from the reference or least-degraded condition, and the comparison with conventional baselines. Include enough detail about data splits and search settings for the experiment to be repeated.

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
