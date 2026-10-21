---
title: Data
description: Prepare image batches or read datasets using Kartezio's data helpers.
---

# Data

The trainer accepts `x` and `y` batches. The project type aliases define a batch as a list of items, where each item is itself a list of NumPy arrays. For a single-channel image task, an item contains one image array.

## In-memory batches

The built-in helper `one_cell_dataset()` returns a one-image training batch and its target annotation:

```python
from kartezio.utils.dataset import one_cell_dataset

train_x, train_y = one_cell_dataset()

# One item per batch; one image array in each item.
image = train_x[0][0]
target = train_y[0][0]
```

The helper uses `skimage.data.cell()` for the image and creates an annotation by drawing a filled circle into a zero-initialized array. It is a small example dataset, not a general annotation loader.

For your own arrays, preserve the same nested batch structure:

```python
x = [[image_0], [image_1]]
y = [[mask_0], [mask_1]]

elite, history = trainer.fit(100, x, y)
```

Each item in `x` and `y` is a list of arrays. The examples use one array in each item for single-channel images and target masks.

## Reading a dataset from files

`kartezio.data.dataset.read_dataset` reads a CSV-backed dataset:

```python
from kartezio.data.dataset import read_dataset

dataset = read_dataset(
    dataset_path="path/to/dataset",
    x_reader="grayscale",
    y_reader="labels",
)

train_x, train_y = dataset.train_xy
test_x, test_y = dataset.test_xy
```

The CSV defaults to `dataset.csv`. The reader splits rows using the `set` column with values `training` and `test`, and reads input and label paths from the `input` and `label` columns.

Supported reader names in the implementation include:

| Argument | Accepted string values |
| --- | --- |
| `x_reader` | `"rgb"`, `"grayscale"`, `"tiff_mono_3d"`, `"tiff_labels_3d"` |
| `y_reader` | `"labels"`, `"imagej"`, `"tiff_labels_3d"`, `"polyhedron"` |

The function also accepts reader instances derived from `DataReader`. A file-based dataset must contain the columns required by its selected readers; `visual` is an optional column used for previews.

Set `preview=True` in `read_dataset` to request preview images for the loaded split. The implementation writes these into a `__preview__` directory associated with the dataset directory.
