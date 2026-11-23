# Exercise 2: Storing Artifacts

## Problem

A developer on your team has created a workflow that generates a text file containing the first 10 prime numbers. They also wrote a small script which will verify that the numbers in the file indeed prime numbers.

However, this developer does not know how to share that file between jobs and reach out to you for help.

Use your knowledge of workflow artifacts to help the developer store the file in a way that it can be accessed by the script in the next job.

The workflow [.github/workflows/primes-generator-verifier.yaml](.github/workflows/primes-generator-verifier.yaml) currently looks like this:

```yaml
name: "Lesson 3 - Exercise 2"

on:
  workflow_dispatch:

jobs:
  generate-primes:
    runs-on: ubuntu-latest
    steps:
      # ----------------------------------
      # Do not modify the code in the following step
      # ----------------------------------
      - name: Generate the first 10 prime
        run: |
          import math

          # The cursor
          count = 3

          # The set that contains the prime numbers
          primes = set([1, 2])

          # Generate prime numbers until we have a total of 10
          # in the primes set
          while len(primes) < 10:
            isprime = True
            
            for x in range(2, int(math.sqrt(count) + 1)):
              if count % x == 0: 
                isprime = False
                break
            
            if isprime:
              primes.add(count)
            
            count += 1

          # Print the primes set
          print(primes)

          # Store the primes set in primes.txt
          with open('primes.txt', 'w') as f:
            for prime in primes:
              f.write(str(prime) + '\n')
        shell: python
      # ----------------------------------
      # Do not modify the code above this line
      # ----------------------------------

      # Add your solution below this line
      # ----------------------------------
      # ...

  verify-primes:
    runs-on: ubuntu-latest
    needs: generate-primes
    steps:
      # Add your solution below this line
      # ----------------------------------
      # ...

      # ----------------------------------
      # Do not modify the code in the following step
      # ----------------------------------
      - name: Verify the generated primes
        run: |
          # Read the primes.txt file
          with open('primes.txt', 'r') as f:
            primes = set([int(line.strip()) for line in f.readlines()])

          # The expected primes
          expected_primes = {1, 2, 3, 5, 7, 11, 13, 17, 19, 23}

          # Check if the generated primes are correct
          assert primes == expected_primes
        shell: python
      # ----------------------------------
      # Do not modify the code above this line
      # ----------------------------------
```

You need to add a step to the `generate-primes` job to store the `primes.txt` file as an artifact. Then, you need to add a step to the `verify-primes` job to download the artifact and use it to verify the generated prime numbers.