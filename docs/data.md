---
title: Benchmark and Data Protocol
description: Benchmark context and the proposed separation of optimization and evaluation conditions.
---

# Benchmark and Data Protocol

## Benchmark

The implementation uses the hardware-items benchmark by Khan, Mikut, and Reischl. It contains 728 grayscale images: 4 scenes, 13 shading levels, and 14 noise levels. Each image is 1000 × 1500 pixels, and each scene has a ground-truth segmentation mask. The benchmark files are not included in the code repository; download them from the [SourceForge dataset page](https://sourceforge.net/projects/gait-cad/files/Benchmarks/hardware_items/) and place them under `hardware_items/data/` and `hardware_items/truth/`.

Input TIFF filenames encode the scene, shading, and noise indices, for example `benchdata_01_02_03.tif`. Noise levels follow the benchmark definition, with variance 125 × (noise index − 1). Ground truth is stored per scene in MATLAB files and reused across that scene's shading/noise conditions.

## Data validation and ground-truth preparation

The repository builds a manifest and validates expected files, image dimensions, data types, annotation alignment, and experiment splits. Ground-truth masks are converted to foreground/background, resized from 3456 × 5184 to 1000 × 1500 using area averaging, and binarized at 0.5. Object counts use 8-connected components. The audited component counts are 27, 32, 51, and 58 across the four scenes; scene 3 has one touching pair, so its component count differs by one from the benchmark's tabulated object count of 52. The implementation intentionally does not apply morphological opening to ground truth.

The default image normalization clips to the 2nd and 98th Hazen percentiles, rescales intensities to [0, 1], and inverts polarity so dark objects become bright. Constant or near-constant images map to zeros. Details are defined in the repository's [frozen benchmark protocol](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/docs/benchmark_protocol.md).

## Scene-held-out split protocol

The implemented split helper supports four leave-one-scene-out rotations. In each rotation, three scenes are used for training and the remaining scene is held out across all 182 shading/noise combinations. Two 9-condition training subsets are defined:

- **Quality-only:** the 3 × 3 combination of shading levels 1–3 and noise levels 1–3.
- **Robustness-aware sampling:** nine combinations spanning shading levels 1, 7, and 13 and noise levels 1, 7, and 14.

The 13-image robustness series for each scene uses shading `b = 1…13` with noise `n = b + 1`. The data audit reports 728 valid images, aligned masks, and no overlap between training and held-out scenes. The split code is implemented; full CGP experiments using these benchmark splits are still planned.

## Reproducibility

Record the benchmark source, manifest, split definition, preprocessing configuration, and any exclusions with every run. The repository's [data audit report](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/docs/data_audit_report.md) records the current validation results. Interpret conclusions only within the benchmark conditions actually evaluated.
