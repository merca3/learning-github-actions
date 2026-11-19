# Exercise 4: Concurrency groups

## Problem

You have a workflow containing a `release` job. This job is responsible for creating a release and publishing it to GitHub. This job creates a "canary" release whenever changes are pushed to the `main` branch. Since the canary release is a non-breaking change, it doesn't matter if it is published any number of times.

However, you also have `releases/*` branches. Each release branch is responsible for creating a "stable" release. The stable release is a breaking change, and you want to ensure that only one stable release is published at a time.

The workflow [.github/workflows/ci.yml](.github/workflows/ci.yml) currently looks like this:

```yaml
name: "Lesson 2 - Exercise 4"

on:
  push:
    branches:
      - main
      # Match the release branches of the format: releases/1, releases/2, etc.
      - "releases/[0-9]"

# Add your solution below this line

jobs:
  release:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - name: Build and release
        run: |
          echo "Building branch: ${{ github.ref }} for OS: ${{ matrix.os }}"
          sleep 10
```

You need to add a concurrency group at the workflow level, to that existing workflow runs for the `releases/*` branches are canceled if a new run is triggered while a previous run is still in progress.

Hint: you can use any expression that evaluates to a Boolean as the value of the `cancel-in-progress` property. Refresh your knowledge of functions and expressions by revisiting [the docs](https://docs.github.com/en/actions/learn-github-actions/expressions#functions).