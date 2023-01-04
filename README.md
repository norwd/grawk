# grawk

*Grok stuff with Grawk!*

## Usage

Grawk is AWK for GitHub Action Pipelines.
It allows running AWK programs in workflow files, imagine setting `shell: awk` in a `run:` step.

### Basic Setup

```yaml
---

name: Run Awk Script
on: push

jobs:
  grawk:
    name: Run Grawk
    runs-on: ubuntu-latest
    steps:
      - name: Run Grawk
        uses: norwd/grawk@v1
        with:
          program: |
            BEGIN {
              print "Hello World!"
            }
```

### Inputs

#### `program`

The text of the awk program.

#### `program-file`

The pathname of the file containing an awk program.

#### `input`

The input to the awk program.

#### `input-file`

The pathname of the file containing the input to the awk program.

#### `separator`

The input field separator.

### Outputs

#### `result`

The result of the awk script.
