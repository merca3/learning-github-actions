# Exercise 1: Workflow Commands

## Problem

One of your colleagues has a workflow that fetches data from an API endpoint, then counts the number of words in the `title` and `body` of the response. The workflow echoes these counts in the logs, but that's not the best way to display this information.

Your task is to modify the workflow to use workflow commands to display the word counts in the summary page of the workflow run. You can use markdown to display the information in a more readable format.

The workflow [.github/workflows/ci.yaml](.github/workflows/ci.yaml) currently looks like this:

```yaml
name: "Lesson 4 - Exercise 1"

on:
  workflow_dispatch:

jobs:
  fetch-api-data:
    runs-on: ubuntu-latest
    steps:
      - name: Save API response to a file
        run: |
          curl https://jsonplaceholder.typicode.com/posts/1 > response.json

          # Count the number of words in the title and body
          title_words_count=$(cat response.json | jq -r '.title' | wc -w | xargs)
          body_words_count=$(cat response.json | jq -r '.body' | wc -w | xargs)

          # Print the word count
          echo "Title word count: $title_words_count"
          echo "Body word count: $body_words_count"

          ## Add your answer below this line
```
