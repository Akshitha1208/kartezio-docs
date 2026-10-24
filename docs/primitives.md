---
title: Evaluation Plan
description: Segmentation-quality and robustness measures for comparing evolved and conventional pipelines.
---

# Evaluation Plan

## Segmentation quality

Compare each predicted foreground mask with its ground-truth mask using overlap measures:

| Measure | Definition | Interpretation |
| --- | --- | --- |
| Dice similarity coefficient | 2 × shared foreground pixels ÷ (predicted foreground pixels + ground-truth foreground pixels) | Overlap between prediction and ground truth; higher is better. |
| Intersection over union (IoU) | Shared foreground pixels ÷ pixels marked foreground in either mask | Shared area relative to the combined foreground area; higher is better. |

Precision and recall can also be reported to distinguish false-positive from false-negative errors.

## Robustness

A metric on one image condition measures segmentation quality there, but not robustness by itself. Report results across the tested illumination and noise conditions. Compare condition-wise performance and its change relative to a clearly identified reference condition.

## Fair comparison

Evaluate evolved pipelines and conventional baselines on the same held-out images and conditions, using the same ground truth and metrics. Keep held-out results separate from optimization and tuning.

## Reproducibility and limitations

Report results across independent evolutionary runs where feasible; a single best run can hide variability. Include the operation set, search settings, data split, and random seeds. Runtime and pipeline complexity may provide additional context, but should be presented only if measured consistently.

Do not treat high optimization fitness as evidence of generalization. Conclusions should reflect the tested benchmark and conditions.
