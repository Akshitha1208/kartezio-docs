---
title: Training, Evaluation & Export
description: Fit the trainer, inspect callbacks, evaluate predictions, and print generated code.
---

# Training, Evaluation & Export

## Fit a trainer

`KartezioTrainer.fit` accepts the number of iterations, input batch, target batch, and an optional list of callbacks:

```python
elite, history = trainer.fit(
    n_iterations=100,
    x=train_x,
    y=train_y,
    callbacks=[],
)
```

It returns the elite individual and the evolution history. The underlying evolution loop may stop before the requested number of iterations if the best fitness reaches exactly zero.

## Configure mutation and strategy

Mutation rates can be set on the trainer:

```python
trainer.set_mutation_rates(node_rate=0.05, out_rate=0.1)
```

The trainer also exposes methods to set mutation decay, behavior, effect, and edge configuration. The exact type accepted by each method depends on its corresponding implementation in `kartezio.mutation`.

## Callbacks

Callbacks observe evolution events. The `frequency` parameter controls how often ordinary events are processed; forced events are still delivered.

The repository provides, among others:

- `CallbackVerbose(frequency=1)` to print generation fitness and timing information.
- `CallbackSaveElite(...)` to serialize elite and model component information as JSON when a new parent is reported.
- `CallbackSaveScores(...)` to record train/test fitness values and save a plot.
- `CarbonCallback()` to track and print emissions data.

Pass callback instances in `fit(..., callbacks=[...])`. The basic example uses `CallbackVerbose`.

## Evaluate the elite

After fitting, `trainer.evaluate(x, y)` runs prediction on the supplied inputs and batches the result with the configured fitness function:

```python
score = trainer.evaluate(train_x, train_y)
print(score)
```

Use data not used during fitting when you want a separate evaluation. The API does not split data automatically.

## Print a Python class

The trainer can generate a Python class for its elite individual:

```python
trainer.print_python_class("CellSegmenter")
```

This method prints code to standard output. Copy the printed class into a `.py` file if you want to save or reuse it; `print_python_class` itself does not write a file.
