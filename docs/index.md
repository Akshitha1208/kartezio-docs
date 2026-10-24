---
title: Project Overview
description: Project documentation for evolving illumination-robust classical image-processing pipelines for image segmentation.
---

# Evolutionary Generation of Illumination-Robust Classical Image-Processing Pipelines for Image Segmentation

This project investigates whether evolutionary computation can automatically assemble classical image-processing operations into segmentation pipelines that remain useful when illumination and background noise vary.

## Problem

Classical operations such as filtering, thresholding, edge detection, and morphology are interpretable and computationally practical. However, their order and parameter settings are often chosen manually, and a pipeline that works in favourable lighting may perform poorly under shadows, uneven illumination, contrast changes, or noise.

## Aim

To generate classical image-processing pipelines using Cartesian Genetic Programming (CGP), then evaluate their segmentation quality and reliability across changing illumination and noise conditions.

## Objectives

- Evolve pipelines composed of classical image-processing operations using the Kartezio framework.
- Evaluate segmentation on benchmark images with controlled shading and background-noise variations.
- Examine performance on conditions held out from optimization.
- Compare evolved pipelines with conventional image-processing approaches.

## Approach at a glance

Candidate pipelines are represented as computational graphs. Evolution uses segmentation performance to select and improve candidates. The resulting pipelines are assessed against ground-truth masks using overlap metrics and compared across image conditions.

## Project scope

The work focuses on classical image-processing operations, CGP-based pipeline generation, segmentation, and empirical evaluation under varying imaging conditions. It evaluates an established evolutionary approach rather than proposing a new evolutionary algorithm. See the [methodology](training.md) and [evaluation plan](primitives.md) for details.

## Team

- D Lakshmi Akshitha
- Kolla Akshara
- Bhuvaneshwari D
- Pranathi Nibhanipudi

Explore the [research background](concepts.md), [project scope and questions](quickstart.md), and [benchmark and data protocol](data.md).
