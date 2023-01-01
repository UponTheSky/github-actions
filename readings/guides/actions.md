# Finding and customizing actions

## Overview
- the same repo of the workflow file
- any public repo
- a published docker container image

## Browsing Marketplace actions in the workflow editor

## Adding an action to your workflow
- actions referenced = dependencies in the dependency graph
  - Adding an action from GitHub Marketplace
  - Adding an action from the same repository(defined as a `yml` file)
  - Adding an action from a different repository: `{owner}/{repo}@{ref}` syntaxs
  - Referencing a container on Docker Hub

## Using release management for your custom actions
- you should indicate the version of the action you'd like to use
  - Using tags
  - Using SHAs
  - Using branches

## Using inputs and outputs with an action
- an action may require inputs and generate outputs
- `action.yml` 