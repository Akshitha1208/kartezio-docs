---
title: Installation
description: Install Kartezio from its source checkout and run this documentation site.
---

# Installation

Kartezio requires Python 3.11 or newer, as declared in the project's `pyproject.toml`.

## Install Kartezio

From the root of the Kartezio source checkout, install the package in editable mode:

```powershell
python -m pip install -e .
```

The project metadata declares the runtime dependencies, which pip installs with the package. The development dependency group contains `pytest` and `coverage`.

To install that group with `uv`, run this from the project root:

```powershell
uv sync --group dev
```

## Install the documentation tools

From this documentation folder, install MkDocs, its Material theme, and the Markdown extensions used by the site:

```powershell
python -m pip install mkdocs mkdocs-material pymdown-extensions
```

## Preview or build the site

Run these commands from this documentation folder:

```powershell
mkdocs serve
```

MkDocs serves the site locally and rebuilds it when documentation files change. To create the static site:

```powershell
mkdocs build --strict
```

The generated site is written to `site/` inside this documentation folder.
