---
title: Preview and Publish This Site
description: Run the project documentation locally and publish updates with GitHub Pages.
---

# Preview and Publish This Site

This repository contains the project's documentation website. It does not contain the image-processing experiment code or provide commands to run the segmentation experiments.

## Preview locally

Install the documentation dependencies:

```powershell
python -m pip install mkdocs-material
```

From the repository root, start the local preview:

```powershell
mkdocs serve
```

Open `http://127.0.0.1:8000` in a browser. Stop the preview with `Ctrl+C`. To check that the site builds:

```powershell
mkdocs build --strict
```

## Publish updates

The GitHub Actions workflow builds and deploys the site when changes are pushed to `main`. In the GitHub repository, configure **Settings → Pages → Build and deployment → Source** as **GitHub Actions**.

Edit the Markdown pages in `docs/`, then commit and push the changes:

```powershell
git add docs
git commit -m "Update project documentation"
git push origin main
```

Check the **Actions** tab for the build and deployment status. Once the workflow succeeds, GitHub Pages serves the updated documentation at the repository's Pages URL.
