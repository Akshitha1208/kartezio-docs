---
title: Implementation Details
description: Code structure, data flow, reproducibility commands, implemented features, and remaining experiments.
---

# Implementation Details

The experiment code is maintained in the [Kartezio illumination-robustness repository](https://github.com/Pranathi-N-47/kartezio-illumination-robustness). This documentation site is a separate repository.

## Code structure

| Area | Implementation |
| --- | --- |
| Benchmark data | [Data modules](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/tree/main/projects/illumination_robustness/data) load TIFF images and scene-level MATLAB annotations, build manifests, and define leakage-checked scene/condition splits. |
| Preprocessing | [Preprocessing modules](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/tree/main/projects/illumination_robustness/preprocessing) normalize image intensities and provide binary-mask cleanup operations. |
| Candidate operations | [Primitive modules](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/tree/main/projects/illumination_robustness/primitives) extend Kartezio's matrix library with project operations, including Otsu thresholding and border clearing used in the pilot. |
| Evolution | [Fitness module](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/projects/illumination_robustness/evolution/fitness.py) adapts the pilot's `Q_seg` score to Kartezio's minimizing loss convention. |
| Evaluation | [Evaluation modules](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/tree/main/projects/illumination_robustness/evaluation) implement Dice, IoU, object-count and paper-derived segmentation scores, robustness summaries, and Otsu/Sobel baselines. |
| Experiment scripts | [Scripts](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/tree/main/scripts) build and validate the data manifest, generate synthetic fixtures, run baselines, and execute the pilot. |

## Data-to-result flow

```text
Hardware-items TIFFs + scene annotations
                 |
      Manifest and data validation
                 |
    Ground-truth alignment and image normalization
                 |
   +-------------+-------------------+
   |                                 |
Fixed Otsu/Sobel baselines     Synthetic CGP pilot
   |                                 |
Dice, IoU, Q_seg, R_seg        Pipeline + pilot metrics
   +---------------+-----------------+
                   |
 Benchmark-scale CGP experiments (planned)
```

## Run the implementation

Run these commands from the root of the [project code repository](https://github.com/Pranathi-N-47/kartezio-illumination-robustness). The project requires Python 3.11 or newer and uses `uv` with the committed lock file:

```powershell
uv sync --group dev
```

The benchmark is not bundled in the repository. Download it separately and arrange the files as `hardware_items/data/` and `hardware_items/truth/`. Then build and check the manifest:

```powershell
uv run python scripts\build_manifest.py
uv run python scripts\validate_data.py --benchmark-root hardware_items
```

Run a small fixed-baseline check:

```powershell
uv run python scripts\run_baselines_python.py --mode smoke --benchmark-root hardware_items
```

Run the synthetic CGP pilot without requiring the benchmark files:

```powershell
uv run python scripts\generate_pilot_fixtures.py --output-dir results\pilot_fixtures --seed 42
uv run python scripts\run_pilot_test.py --generations 50 --nodes 16 --seed 42 --fixture-dir results\pilot_fixtures
```

Run the project-specific tests:

```powershell
uv run pytest tests\test_illumination_robustness -q
```

The pilot writes its summary, pipeline description, fitness history, and predictions under `results\pilot_test\` by default. Baseline outputs are written under `results\`.

## Implemented and pending work

**Implemented:** benchmark manifest and data audit; ground-truth alignment and preprocessing; segmentation and robustness metrics; Python and MATLAB-compatible Otsu/Sobel baseline workflows; synthetic fixture generation; pilot CGP evolution and pipeline export; deterministic leave-one-scene-out split definitions.

**Still planned:** final benchmark-scale CGP runs, final quality-only versus robustness-aware fitness experiments, repeated-seed comparisons, and analysis of evolved-pipeline robustness, complexity, and failure cases. The split definitions and pilot workflow exist, but they do not mean those final evolutionary experiments have already been completed.

## Pilot results and limitations

The documented pilot sweep uses one seed, a 16-node limit, and synthetic fixtures. Its unseen synthetic test set contains only four fixtures, so the pilot demonstrates that training, selection, evaluation, and export work together; it does not establish generalization to real benchmark scenes.

The corrected full Python baseline evaluated both Otsu and Sobel on all 728 benchmark images. The score summary and comparison limits are on the [evaluation plan](primitives.md) page and in the repository's [baseline reproduction report](https://github.com/Pranathi-N-47/kartezio-illumination-robustness/blob/main/docs/baseline_reproduction_report.md). Python Sobel is an approximation of MATLAB Sobel, and the reported `Q_seg`/`R_seg` exclude the original paper's classification term.
