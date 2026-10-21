---
title: Quick Start
description: Train and evaluate a single-objective image-processing pipeline.
---

# Quick Start

This example follows the project's basic training example. It uses the included one-cell sample data, a matrix-operation library, threshold output endpoint, and IoU fitness.

## Train a pipeline

```python
from kartezio.callback import CallbackVerbose
from kartezio.core.endpoints import EndpointThreshold
from kartezio.core.fitness import IoU
from kartezio.evolution import KartezioTrainer
from kartezio.primitives.matrix import default_matrix_lib
from kartezio.utils.dataset import one_cell_dataset

train_x, train_y = one_cell_dataset()

trainer = KartezioTrainer(
    n_inputs=1,
    n_nodes=10,
    libraries=default_matrix_lib(),
    endpoint=EndpointThreshold(128),
    fitness=IoU(),
)
trainer.set_mutation_rates(node_rate=0.05, out_rate=0.1)

elite, history = trainer.fit(
    100,
    train_x,
    train_y,
    callbacks=[CallbackVerbose(frequency=1)],
)

score = trainer.evaluate(train_x, train_y)
print("Evaluation:", score)
print("Elite:", elite)
print("History entries:", len(history))

trainer.print_python_class("CellSegmenter")
```

`one_cell_dataset()` returns a batch containing one image and one annotation. The example runs training on that batch and evaluates on the same data; it does not measure performance on a separate test set.

`CallbackVerbose` prints progress during evolution. `print_python_class` prints the generated class code to the console; it does not save a Python file automatically.

## Next steps

- Review [Core Concepts](concepts.md) to understand the components in the trainer.
- See [Data](data.md) for the expected batch structure and file-based dataset reader.
- See [Training, Evaluation & Export](training.md) for the `fit` return values and callback behavior.
