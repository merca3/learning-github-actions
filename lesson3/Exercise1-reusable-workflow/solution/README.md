# Exercise 1: Reusable Workflows

## Problem

You have a workflow that you want to reuse in other repositories. You want to avoid copying and pasting the workflow file in each repository.

The current workflow definition:

```yaml
name: "Lesson 3 - Exercise 1"

on:
  workflow_dispatch:
    inputs:
        actor:
            description: "The person who will receive the greeting"
            required: true

jobs:
  greeting:
    runs-on: ubuntu-latest
    steps:
      - name: Greetings message
        run: |
          echo "Well done: $ACTOR you've implemented reusable workflows successfully!"
        env:
          ACTOR: ${{ inputs.actor }}
```

Modify this workflow so that it becomes a reusable workflow. Then create a new repository and use this workflow in that repository.

You should see a successful run of the caller workflow in the repository where it is hosted.

### Hints

1. Make sure both repositories are public so that you don't have to worry about organization-level policy settings.
1. Target the called workflow at the `main` branch. However, you're strongly encourage to use other refs:
   1. Use a tag
   1. Use a non-default branch
   1. Use a specific commit SHA
