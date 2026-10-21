---
title: Kartezio
description: Documentation for the Kartezio image-processing and Cartesian Genetic Programming framework.
---

# Kartezio documentation

Kartezio is a Python framework for building image-processing pipelines with Cartesian Genetic Programming (CGP). This documentation explains how to install the package, prepare data, configure a trainer, and work with the implemented components.

[Install Kartezio](installation.md){ .md-button .md-button--primary }
[Train your first pipeline](quickstart.md){ .md-button }

## Project overview

This project investigates using evolutionary computation to generate classical image-processing pipelines for image segmentation. The goal is to assess not only segmentation quality, but also how reliably pipelines perform when illumination and noise vary.

### Problem statement

Manually choosing and tuning the order of filtering, thresholding, edge-detection, and morphological operations can require repeated experimentation. Pipelines that work well under favourable image conditions may also lose performance when illumination or noise changes.

### Objectives

- Generate image-segmentation pipelines with Cartesian Genetic Programming (CGP) using Kartezio.
- Evaluate segmentation quality and reliability under varying illumination and noise.
- Test generalization to illumination and noise conditions not used during optimization.
- Compare generated pipelines with conventional image-processing approaches.

### Methodology

Represent candidate pipelines as computational structures and evolve them using segmentation performance to guide selection. Use the Khan, Mikut, and Reischl benchmark, separating optimization conditions from held-out illumination and noise conditions. Evaluate masks with Dice and intersection over union (IoU), then compare generated pipelines with conventional methods.

### Team

- D Lakshmi Akshitha
- Kolla Akshara
- Bhuvaneshwari D
- Pranathi Nibhanipudi

## Documentation

<div class="grid cards" markdown>

-   :material-book-open-page-variant:{ .card-icon } **User guide**

    ---

    Learn how the trainer and graph work, supply image data, and run training and evaluation.

    [:octicons-arrow-right-24: Open the user guide](concepts.md)

-   :material-api:{ .card-icon } **Component reference**

    ---

    Review the built-in primitive libraries, endpoints, fitness functions, and extension points.

    [:octicons-arrow-right-24: Browse components](components.md)

-   :material-download:{ .card-icon } **Getting started**

    ---

    Set up the package and follow a complete single-objective training example.

    [:octicons-arrow-right-24: Get started](installation.md)

</div>

## How Kartezio is assembled

A `KartezioTrainer` combines an operation library, an endpoint, and a fitness function. The trainer evolves a graph using input and target batches, then exposes the elite individual for evaluation or code generation.

```text
Input images  ->  CGP graph and primitives  ->  Endpoint  ->  Predictions
Targets and predictions  ------------------------------->  Fitness score
```

## Suggested reading

1. [Install Kartezio](installation.md).
2. Follow the [Quick Start](quickstart.md).
3. Explore [data loading](data.md), [core concepts](concepts.md), and the [component reference](components.md).

The examples and feature descriptions on this site are based on the checked-out project implementation. The training workflow documented here uses one fitness component.

Kartezio's research background and publication citation are covered in [Core Concepts](concepts.md#research-background). The project is introduced in the 2023 paper [“Evolutionary design of explainable algorithms for biomedical image segmentation”](https://www.nature.com/articles/s41467-023-42664-x).
