# Exercise 2: GitHub Pages

## Problem

The SkyMaps API SDK team has created a new GitHub Pages site to host the documentation for the SDK. The entire documentation is in the [index.html](index.html) file in the `main` branch.

Your task is to create a workflow that deploys the documentation to GitHub Pages whenever a new commit is pushed to the `main` branch.

1. Make sure to enable GitHub Pages in the repository settings and use the GitHub Actions workflow as the deployment option.
2. Create a new workflow file in the `.github/workflows` directory called: `pages.yaml` that will publish any changes pushed to the `main` branch to the GitHub Pages site.