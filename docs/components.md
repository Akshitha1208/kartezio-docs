---
title: Components
description: Built-in operations, endpoints, and fitness components.
---

# Components

Kartezio composes a trainer from three main component types:

1. **Primitive libraries** provide operations available to graph nodes.
2. **Endpoints** process graph outputs to produce model predictions.
3. **Fitness components** score predictions against target data.

## Primitive libraries

`default_matrix_lib()` constructs the built-in matrix-operation library. The matrix primitives include pixel-wise arithmetic and bitwise operations, filtering, morphology, thresholding, and feature transforms. The complete operation list, including the scalar-enabled variants and primitives not included in the default library, is in the [Primitive Library reference](primitives.md).

```python
from kartezio.primitives.matrix import default_matrix_lib

libraries = default_matrix_lib()
```

The helper also accepts `use_scalars=True`. This substitutes scalar-input variants for selected matrix operations. The project's advanced training example pairs that configuration with `default_scalar_lib(include_matrix=True)`.

`default_scalar_lib()` provides constants, scalar arithmetic and comparisons. With `include_matrix=True`, it also adds image-derived values such as min, max, mean, median, coverage, and connected-component count.

## Endpoints

An endpoint defines how graph outputs become predictions. The endpoint's input signature also informs the graph adapter of the number and kind of outputs it needs to produce.

Examples implemented in `kartezio.core.endpoints` include:

| Endpoint | Behavior |
| --- | --- |
| `NoEndpoint` | Returns the supplied values unchanged. |
| `ResizeUp` | Upscales a matrix by a factor of two using Lanczos interpolation. |
| `EndpointThreshold` | Applies threshold-to-zero processing to one matrix input; values below the threshold become zero and other values are retained. |
| `ToLabels` | Converts the input to connected-component labels. |
| `ThresholdWatershed` | Applies watershed using one or two intensity thresholds. |
| `LocalMaxWatershed` | Finds local maxima and uses them as watershed markers. |
| `DistanceWatershed` | Computes a distance transform, finds markers, and applies watershed. |
| `ThresholdLocalMaxWatershed` | Thresholds an image before local-maxima watershed. |
| `MarkerControlledWatershed` | Applies marker-controlled watershed using two matrix inputs. |
| `HoughCircle` | Detects circles and converts them to labels. |
| `EndpointSubtract` | Subtracts the second matrix from the first. |
| `EndpointSimple` | Returns its matrix input unchanged. |
| `RescaleUp` | Applies the configured resize preprocessing to the output. |
| `RawLocalMaxWatershed3D` | Applies the implemented 3D watershed operation to a stack. |

The basic example configures `EndpointThreshold(128)` for one matrix output. This endpoint is not itself a binary threshold: it retains above-threshold pixel values. With the default `IoU` fitness, positive prediction values are subsequently treated as foreground.

`EndpointWatershed` and `PeakedMarkersWatershed` are abstract base classes, not endpoints to instantiate directly.

## Fitness functions

Fitness components are defined in `kartezio.core.fitness`:

| Component | Constructor options | Implemented score |
| --- | --- | --- |
| `IoU` | `reduction="mean"`, `balance=None` | One minus intersection-over-union by default. |
| `AveragePrecision` | `reduction="mean"`, `threshold=0.5`, `iou_factor=0.0` | One minus Cellpose average precision, optionally combined with scaled IoU. |
| `MSE` | `reduction="mean"` | Mean squared error. |

The fitness base class accepts a reduction argument. `IoU` supports `balance="sensitivity"`, `"specificity"`, or `"balanced"` in addition to its default behavior.

With default `IoU()` settings, predictions are converted to foreground/background by treating values greater than zero as foreground; the returned score is one minus IoU for each image. `AveragePrecision` returns one minus the Cellpose average-precision result and accepts a threshold and optional IoU contribution. `MSE` returns the mean squared error for each image. The trainer consumes these values as fitness scores.

## Component registry and extensions

Components can be registered with the `register` decorator in `kartezio.core.components`. The repository includes examples for defining a custom primitive and a custom endpoint in `examples/components/`. A custom primitive implements `call`; a custom endpoint also defines its accepted input signature and implements `call`.
