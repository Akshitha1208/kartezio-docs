---
title: Primitive Library
description: Matrix and scalar operations provided by Kartezio's built-in primitive libraries.
---

# Primitive Library

Primitives are the operations that graph nodes can execute. Each primitive declares its input signature, output type, and any integer parameters. This page documents the operations defined by the checked-out source and distinguishes those added by the default library from other implemented classes.

## Default matrix library

`default_matrix_lib()` returns a matrix-output `Library`. The following operations are added by default. With `use_scalars=False` (the default), the listed arithmetic, blur, threshold, erosion, and dilation operations take their non-scalar forms. With `use_scalars=True`, the library uses the scalar-input alternatives shown in parentheses.

### Matrix combination and arithmetic

| Primitive | Behavior |
| --- | --- |
| `Identity` | Returns its matrix input unchanged. |
| `Max`, `Min` | Computes the pixel-wise maximum or minimum of two matrices. |
| `Mean` | Computes a pixel-wise average of two matrices. |
| `Add`, `Subtract`, `Multiply`, `Divide` | Applies the named pixel-wise operation to two matrices. (`AddScalar`, `SubtractScalar`, `MultiplyScalar`, `DivideScalar` use a scalar as the second input.) |
| `AbsDiff` | Computes the pixel-wise absolute difference of two matrices. |
| `Inverse` | Subtracts each pixel value from 255. |
| `InverseNonZero` | Inverts pixel values, then sets inverted 255 values to zero. |
| `Sqrt`, `Pow`, `Exp`, `Log` | Applies the corresponding transform to a matrix and converts the result to the implementation's 8-bit image representation. |

### Bitwise operations

| Primitive | Behavior |
| --- | --- |
| `BitwiseNot` | Applies bitwise NOT. |
| `BitwiseOr`, `BitwiseAnd`, `BitwiseXor` | Applies the named pixel-wise bitwise operation to two matrices. |
| `BitwiseAndMask` | Uses the second matrix as the mask for a bitwise AND of the first matrix. |

### Smoothing and edge processing

| Primitive | Behavior |
| --- | --- |
| `MedianBlur` | Applies a median blur. (`MedianBlurScalar` takes its kernel size from a scalar input.) |
| `GaussianBlur` | Applies a Gaussian blur. (`GaussianBlurScalar` takes its kernel size from a scalar input.) |
| `Laplacian` | Computes a Laplacian response. |
| `Sobel`, `Roberts` | Computes gradient magnitude using the corresponding derivative operator. |
| `Canny` | Applies Canny edge detection using a node parameter as its lower threshold. |
| `Sharpen` | Applies the implementation's sharpening convolution kernel. |
| `GaussianDiff` | Subtracts a Gaussian-blurred image from the input, with a configured offset. |
| `Kirsch` | Computes a compass-gradient response using Kirsch kernels. |
| `Gabor3`, `Gabor5`, `Gabor7`, `Gabor9`, `Gabor11` | Applies Gabor filters at the corresponding kernel size and combines orientations by pixel-wise maximum. |
| `LaplacianOfGaussian` | Applies a Laplacian-of-Gaussian kernel. |

### Morphology and mask operations

| Primitive | Behavior |
| --- | --- |
| `Erode`, `Dilate` | Applies erosion or dilation with an elliptical kernel. (`ErodeScalar`, `DilateScalar` take the kernel size from a scalar input.) |
| `Open`, `Close` | Applies morphological opening or closing. |
| `Gradient` | Computes a morphological gradient. |
| `TopHat`, `BlackHat` | Computes the top-hat or black-hat morphological transform. |
| `HitMiss` | Applies the configured hit-or-miss kernel. |
| `Fill` | Fills holes using the project image helper. |
| `RmSmallObjects`, `RmSmallHoles` | Removes foreground objects or holes smaller than the node parameter. |
| `Threshold` | Applies threshold-to-zero using a node parameter. (`ThresholdScalar` uses a scalar input for the threshold.) |
| `InRange` | Keeps original pixel values that are between two node parameters and zeros other pixels. |
| `BinaryInRange` | Produces a binary range mask from two node parameters. |

### Scale, distance and normalization

| Primitive | Behavior |
| --- | --- |
| `PyrUp`, `PyrDown` | Applies a pyramid up/down transform, then resizes the result to the original dimensions. |
| `DistanceTransform` | Computes and normalizes an L2 distance transform to the 0–255 range. |
| `Normalize` | Normalizes matrix values to the 0–255 range. |

## Default scalar library

`default_scalar_lib()` adds `Const` and scalar arithmetic/comparison primitives. With `include_matrix=True`, it also adds matrix-to-scalar measurements.

| Primitive | Behavior |
| --- | --- |
| `Const` | Returns the node parameter as a scalar. |
| `AddValues`, `SubtractValues`, `MultiplyValues` | Applies scalar arithmetic, clamping the result to the 0–255 range. |
| `PowValue`, `SqrtValue` | Squares or takes the square root of a scalar. |
| `MultiplyBy2`, `DivideBy2` | Multiplies or integer-divides a scalar by two. |
| `MinScalars`, `MaxScalars`, `MeanScalars` | Combines two scalar values using the named operation. |
| `LessThan`, `GreaterThan` | Returns 1 when the comparison is true, otherwise 0. |
| `MaxValue`, `MinValue`, `MeanValue`, `MedianValue` | Measures the corresponding statistic of a matrix. Requires `include_matrix=True`. |
| `MeanAbsDiff` | Returns the mean absolute difference between two matrices. Requires `include_matrix=True`. |
| `Coverage` | Returns the fraction of nonzero pixels scaled to 0–255. Requires `include_matrix=True`. |
| `CountRegions` | Counts unique connected-component labels, capped at 255. Requires `include_matrix=True`. |

`Skew` and `Kurtosis` are implemented scalar primitives but are not added by `default_scalar_lib()`; their registration in that helper is commented out.

## Implemented but not added to the default matrix library

The following matrix primitives are defined in the source but are not added by `default_matrix_lib()`. They are therefore not available to a trainer that only uses this helper unless the library is explicitly extended. Some appear as commented-out candidates in the helper:

| Primitive | Behavior |
| --- | --- |
| `Pow2` | Applies the square transform (the same operation as `Pow` in the current implementation). |
| `AbsoluteDifference2` | Returns 255 minus the absolute difference of two matrices. |
| `Kuwahara` | Applies the Kuwahara filter. |
| `Embossing` | Applies an emboss convolution kernel. |
| `Denoize` | Applies OpenCV fast non-local-means denoising. |
| `LocalBinaryPattern` | Computes a uniform local binary pattern. |
| `Meijiring`, `Sato`, `Frangi`, `Hessian` | Applies the corresponding vesselness or Hessian-based filter. |

The source defines `Gabor` as the base implementation used by the registered `Gabor3`, `Gabor5`, `Gabor7`, `Gabor9`, and `Gabor11` classes; instantiate those concrete classes instead.

## Extend a matrix library

`Library.add_primitive()` accepts a primitive whose output type matches the library's return type. The project demonstrates creating a custom primitive in `examples/components/create_primitive.py`.

```python
library = default_matrix_lib()
library.add_primitive(YourPrimitive())
```

Here, `YourPrimitive` represents an instance of a registered `Primitive` subclass, as in the project's `examples/components/create_primitive.py`. Its output type must be `DataType.MATRIX` to add it to this matrix library. The primitive reference describes built-ins; constructor and parameter details are authoritative in the corresponding source definitions.

## Source locations

- Matrix primitives: `src/kartezio/primitives/matrix.py`
- Scalar primitives: `src/kartezio/primitives/scalar.py`
- Endpoint components: `src/kartezio/core/endpoints.py`
- Fitness components: `src/kartezio/core/fitness.py`
