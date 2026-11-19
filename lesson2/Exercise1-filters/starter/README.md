# Exercise 1: Event Filters

## Problem

The development team owning this repository have a test suite that takes 1 full hour to run and complete. They use feature branches and everytime they push to these branches, the CI workflow runs the full test suite. This is not ideal as the team would like to have a faster feedback loop.

They have a simple workflow file: [.github/workflows/ci.yml](.github/workflows/ci.yml).

```yaml
name: Continuous Integration

on: push

jobs:
  test:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run test suite
        run: |
          echo "Running tests..."
          sleep 5 # Simulate running tests
          exit 0
```

Your task is to use the `paths` and `branches` filters to optimize the workflow so that the full test suite only runs when changes are pushed to the `main` branch. It should ignore all other branches. The workflow should also ignore all changes to `.md` files.
