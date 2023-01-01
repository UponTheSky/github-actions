# Essential features of GitHub Actions

## Overview
- essential customization techniques for configuring actions

## Using variables in your workflows
- default envvars
- custom environment variables? 

```yml
jobs: 
  example-job:
    steps: 
      - name: Connect to PostgreSQL
        run: node client.js
        env: 
          POSTGRES_HOST: postgres
          POSTGRES_PORT: 5432
```

## Adding scripts to your workflow

```yml
jobs: 
  example-job:
    steps: 
      - run: npm install -g bats
      - run: ./.github/scripts/build.sh # if you want to run a script as an action
        shell: bash
```

## Sharing data between jobs
- artifacts: files that are shared btw jobs in the same workflow
  - associated with the workflow run where they're created, and can be used by another job

```yml
jobs:
  example-job:
    name: Save output
    steps:
      - shell: bash
        run: | # create an file
          expr 1 + 1 > output.log 
      - name: Upload output file
        uses: actions/upload-artifact@v3 # upload a file as an artifact
        with:
          name: output-log-file 
          path: output.log
```

- to download an artifact from a separate workflow run
```yml
jobs:
  example-job:
    steps:
      - name: Download a single artifact
        uses: actions/download-artifact@v3
        with:
          name: output-log-file
```