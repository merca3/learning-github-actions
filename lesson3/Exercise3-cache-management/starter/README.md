# Exercise 3: Cache Management

## Problem

Building up on Exercise 2, the generation of prime numbers is an expensive operation.

You recommended to the developer to check if the `prime.txt` file exists, if it does, the script should exit early and the workflow will rely on the cached file.

Your task is to modify the workflow to save and download `primes.txt` from the cache.

The workflow [.github/workflows/primes-generator-verifier.yaml](.github/workflows/primes-generator-verifier.yaml) currently looks like this:

```yaml
name: "Lesson 3 - Exercise 3"

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
          import os
          import math

          # If the file prime.txt exists in the current path, exit with 0
          if os.path.exists('primes.txt'):
            print('File primes.txt already exists. Exiting...')
            exit(0)

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

      # Add your solution below this line
      # ----------------------------------
      # ...

      - name: Store the generated primes as an artifact
        uses: actions/upload-artifact@v4
        with:
          name: primes
          path: primes.txt

  verify-primes:
    runs-on: ubuntu-latest
    needs: generate-primes
    steps:
      - name: Download the generated primes
        uses: actions/download-artifact@v4
        with:
          name: primes
          path: .

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
```

### Hint

1. Run the workflow twice in a row to see the impact of caching
1. Inspect the output of the step `` it should match the following:

```bash
Cache Size: ~0 MB (211 B)
/usr/bin/tar -xf /home/runner/work/_temp/562274ef-c89f-43ad-89ba-e9b4230b51b2/cache.tzst -P -C /home/runner/work/owner/repo --use-compress-program unzstd
Cache restored successfully
Cache restored from key: primes-3dd83347e40957c7a88eafe86e6a1b1e30ab4321403cf760aee8484f1816869b
```