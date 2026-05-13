# grawk

*Grok[^not-that-grok] stuff with Grawk!*

[^not-that-grok]: No, not that grok!

<img src="https://codeberg.org/norwd/human/raw/branch/main/docs/automatic-logo.svg" height="50" />

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

# I'm Using GitHub Under Protest

My projects are currently hosted on GitHub.
This is not ideal; GitHub is a proprietary, trade-secret system that is not Free and Open Source Software (FOSS).
I am deeply concerned about using a proprietary system like GitHub to develop my FOSS projects.
I have an [open discussion][Leaving GitHub Discussion] where I am planning how to move away from GitHub in the long term.
I urge you to read about the [Give up GitHub] campaign from the [Software Freedom Conservancy] to understand some of the reasons why GitHub is not a good place to host FOSS projects.

If you are a contributor who personally has already quit using GitHub, please [check this resource][Leaving GitHub Discussion] for how to send in contributions without using GitHub directly.

Any use of my projects' code by GitHub Copilot, past or present, is done without my permission.
I do not consent to GitHub's use of any of my projects' code in Copilot.

![Logo of the GiveUpGitHub campaign](https://sfconservancy.org/static/img/GiveUpGitHub.png)

[Leaving GitHub Discussion]: https://github.com/norwd/norwd/discussions/7
[Give up GitHub]: https://GiveUpGitHub.org
[Software Freedom Conservancy]: https://sfconservancy.org
