---
title: Core Concepts
description: The single-objective trainer, graph, and component model.
---

# Core Concepts

Kartezio builds image-processing pipelines as Cartesian Genetic Programming graphs. In the implementation, the graph is decoded using an input count, node count, operation libraries, and an endpoint.

## Research background

Kartezio was introduced in the paper [“Evolutionary design of explainable algorithms for biomedical image segmentation”](https://www.nature.com/articles/s41467-023-42664-x), published in *Nature Communications* in 2023. The work presents Cartesian Genetic Programming as a way to evolve image-processing algorithms whose operations can be inspected, rather than treating the resulting segmentation system as an opaque learned model.

The [KartezioPaper repository](https://github.com/KevinCortacero/KartezioPaper) identifies itself as containing the main scripts used for the article and links to associated datasets and trained models. The paper provides research context; the code checkout documented here is the authority for current package APIs and available operations.

**Citation**

> Cortacero, K. et al. “Evolutionary design of explainable algorithms for biomedical image segmentation.” *Nature Communications* 14, 7112 (2023). [https://doi.org/10.1038/s41467-023-42664-x](https://doi.org/10.1038/s41467-023-42664-x)

```bibtex
@article{cortacero2023evolutionary,
  title={Evolutionary design of explainable algorithms for biomedical image segmentation},
  author={Cortacero, K{\'e}vin and McKenzie, Brienne and M{\"u}ller, Sabina and Khazen, Roxana and others},
  journal={Nature Communications},
  volume={14},
  number={1},
  pages={7112},
  year={2023},
  doi={10.1038/s41467-023-42664-x}
}
```

## The training loop

The basic setup uses `KartezioTrainer` from `kartezio.evolution`:

```python
trainer = KartezioTrainer(
    n_inputs=1,
    n_nodes=10,
    libraries=default_matrix_lib(),
    endpoint=EndpointThreshold(128),
    fitness=IoU(),
)
```

The trainer is initialized with a single fitness component. Its default strategy is `OnePlusLambda`, and evolution evaluates the population, selects the current parent, and continues through generations until the configured iteration count is reached or the best fitness is exactly zero.

## Inputs and graph nodes

- `n_inputs` is the number of input values supplied to the graph. A grayscale image example uses one input.
- `n_nodes` is the number of processing nodes configured for the graph.
- Each library provides operations that can be selected for graph nodes.
- The endpoint consumes the graph's output and returns the final prediction.
- The fitness component compares predicted outputs with the target batch.

The graph adapter derives the number of outputs from the endpoint's input signature. For instance, `EndpointThreshold` accepts one matrix input.

See the [Primitive Library reference](primitives.md) for the operations available in the built-in libraries.

## Fitness and minimization

The built-in fitness implementations return loss-like values used by the evolutionary process:

| Fitness | Implementation behavior |
| --- | --- |
| `IoU` | Computes `1 - IoU` per image by default. |
| `AveragePrecision` | Computes `1 - cellpose_ap`; an optional `iou_factor` adds a scaled IoU fitness value. |
| `MSE` | Computes mean squared error per image. |

The implementation also supports `IoU` balance options of `"sensitivity"`, `"specificity"`, and `"balanced"`. See [Components](components.md) for the relevant constructor parameters.

## Why the pieces are separate

Operation libraries, endpoints, and fitness functions are separate components. This lets a trainer reuse the graph-evolution mechanism with different image operations, output processing, or scoring criteria, while still optimizing one fitness component at a time.
