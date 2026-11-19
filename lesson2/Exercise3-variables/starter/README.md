# Exercise 3: Variables

## Problem

You have a workflow job that misbehaves when it is rerun more than 2 times. You intend to fix this by rewriting the workflow, but for now you need a stopgap solution to prevent the job from running more than 2 times.

The workflow [.github/workflows/ci.yml](.github/workflows/ci.yml) currently looks like this:

```yaml
name: "Lesson 2 - Exercise 3"

on:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Prevent reruns
        run: |
          # Exit if the run number is greater than the maximum number of attempts
          if [ $RUN_ATTEMPT_NUMBER -gt $MAXIMUM_ATTEMPTS ]; then
            echo "Exiting because the run number is greater than the maximum number of attempts"
            exit 1
          fi
        # Add your solution below this line
        # Hint: read more about the github context here: https://docs.github.com/en/actions/learn-github-actions/contexts#github-context
        # Hint: read more about setting job level environment variables here: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idenv

      - name: Run job
        run: |
          echo "Running job"
          sleep 1
```

Define a repository variable `MAXIMUM_ATTEMPTS` with a value of `2` and use it to prevent the `test` job from running more than 2 times. Use the `github` context to get the number of run attempts, and use environment variables to pass these values to the job and the shell script that is doind the check.

If everything is set up correctly, you should be able to rerun the workflow up to 2 times, but not more than that.

Before you start, read more about [rerunning workflows and jobs.](https://docs.github.com/en/actions/managing-workflow-runs/re-running-workflows-and-jobs).