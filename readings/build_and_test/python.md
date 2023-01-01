# Building and testing Python

## Using the Python starter workflow

```yml
name: Python Example

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.7", "3.8"]

    steps:
      - uses: actions/checkout@v3
      - name: Set up Python $${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: $${{ matrix.python-version }}
      - name: Install dependencies
        run: | 
          python -m pip install --upgrade pip
          pip install flake8 pytest
          if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
      - name: Lint with flake8
        run: |
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
          flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
      - name: Test with pytest
        run: |
          pytest

```

## Specifying a Python version
- `setup-python` action on a github-hosted runner
  - uses the tools cache & adds the necessary binaries to `PATH`

### Using multiple Python versions

### Using a specific Python version
- you can use semantic version syntax to fetch the latest minor release
- you can also specify a certain architecture for python(amd, x64, etc.)

### Excluding a version

```yml
jobs:
  build:

    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        python-version: ["3.7", "3.8", "3.9", "3.10", pypy-2.7, pypy-3.8]
        exclude:
          - os: macos-latest
            python-version: "3.7"
          - os: windows-latest
            python-version: "3.7"
```

### Using the default Python version
- using `setup-python` is recommended

## Installing dependencies

### Requirements file

### Caching Dependencies
- you can cache and restore the dependencies using the `setup-python` action
- you can also use cache action

- example:

```yml
steps:
- uses: actions/checkout@v3
- uses: actions/setup-python@v4
  with:
    python-version: '3.10'
    cache: 'pip' # either pip, pipenv, or poetry
- run: pip install -r requirements.txt
- run: pip test
```

```yml
- uses: actions/cache@v3
  with:
    path: ~/.cache/pip # this is for ubuntu
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-pip-
```

## Testing your code

### Testing with pytest and pytest-cov

### Using Flake8 to lint code
- `continue-on-error: true`

### Running tests with tox

## Packaging workflow data as artifacts
- in order to save log files, core dumps, test results, or screenshots

```yml
- name: Upload pytest test results
  uses: actions/upload-artifact@v3
  with:
    name: pytest-results-${{ matrix.python-version }}
    path: junit/test-results-${{ matrix.python-version }}.xml
  if: ${{ always() }}
```

## Publishing to package registries
- for future reference: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python#publishing-to-package-registries
