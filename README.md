# grawk

*Grok stuff with Grawk!*

<img src="/../../../../norwd/human/blob/main/docs/automatic-logo.svg" height="50" />

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

### Advanced Setup

```yaml
---

name: Run Awk Script
on: push

jobs:
  process-students:
    name: Process Student Records
    runs-on: ubuntu-latest
    steps:
      - name: Download Student Records
        shell: bash
        run: |
          wget https://raw.githubusercontent.com/norwd/grawk/v1/.github/testing/students.csv

      - id: students-named-alex
        name: Find Students Named Alex
        uses: norwd/grawk@v1
        with:
          input-file: students.csv
          program: |
            $1 == "Alex" { # Format is FIRST_NAME, LAST_NAME, AGE, SUBJECT
              print $2 ", " $1 ": " $3 " year old " $4 " student"
            }

      - name: Print Found Students
        shell: bash
        run: echo "$STUDENTS"
        env:
          STUDENTS: ${{ steps.students-named-alex.outputs.results }}
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
