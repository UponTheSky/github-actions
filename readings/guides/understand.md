# Understanding GitHub Actions

## Overview
- a CI/CD platform
- automation of build, test, and deployment pipeline
- provides cloud-based virtual machine to run workflows, but we can host our own runners in a data center or 
any other cloud infrastructure

## The components of GitHub Actions
- an **event** occurs => a **workflow** is triggered
- one **workflow** has one or more **jobs** that are run either in sequential order or in parallel, and
each job runs inside its own VM runner or inside a container
- each job has one or more **steps**

### Workflows
- a configurable automated process that run one or more jobs
- defined by a yaml file in `.github/workflow` directory, and triggered by an event, schedule, or just manually
- a repo can have mutiple workflows, and one workflow can refer another

### Events
- a special activity in a repository that triggers a workflow run

### Jobs
- a set of steps in a workflow that execute on the same runner
  - each step is either a shell script or an **action**
  - steps are executed in order and are dependent on each other
  - since each step is executed on the same runner, those steps share data

- jobs can be configured to have depenencies on each other

### Actions
- a custom application for the Github Actions that performs a complex but frequently repeated task

### Runners
- a server that runs your workflows when they're triggered
- each runner can run a single job at a time, and each workflow is executed in a fresh, newly-provisioned VM

## Create an example workflow

## Understanding the workflow file

```yml
name: <workflow_name> # the name of the workflow
run-name: <workflow_run_name> # the name for the workflow runs generated from the workflow

on: [push] # specifies the trigger for this workflow

jobs: # groups together all the jobs that run in the current workflow
  check-bats-version: # the name of a job
    runs-on: ubuntu-latest # the name of the runner
    steps: # groups together all the steps in this job
      - uses: actions/checkout@v3 # uses => this step will run actions/checkout action of version 3
      - uses: actions/setup-node@v3 # install node
        with: 
          node-version: '14'
      - run: npm install -g bats # run => execute a command on the runner
      - run: bats -v

```

### Visualizing the workflow file
- when your workflow is triggered, a **workflow run** is created that executes the workflow

## Viewing the activity for a workflow run
