# Super Linter Reusable Workflow

[![Experimental](https://img.shields.io/badge/Stability-Experimental-red.svg)](https://github.com/equinix-labs/equinix-labs/blob/main/experimental-statement.md#experimental-statement)

[![GitHub Super-Linter](https://github.com/equinix-labs/metal-action-runner/actions/workflows/superlinter.yml/badge.svg)](https://github.com/marketplace/actions/super-linter)
This is a fork of [bretfisher/super-linter-workflow](https://github.com/BretFisher/super-linter-workflow), further customized for Equinix Labs needs. Please see Bret's original repo for more information and fork and contribute to share with the community.

```yaml
---
# template source: https://github.com/equinix-labs/super-linter-workflow/blob/main/templates/call-super-linter.yaml
name: Lint Code Base

on:
  # run anytime a PR is merged to main or a direct push to main
  push:
    branches: [main]

  # run on any push to a PR branch
  pull_request:

# cancel any previously-started, yet still active runs of this workflow on the same branch
concurrency:
  group: ${{ github.ref }}-${{ github.workflow }}
  cancel-in-progress: true

jobs:
  call-super-linter:
    name: Call Super-Linter

    permissions:
      contents: read # clone the repo to lint
      statuses: write # read/write to repo custom statuses

    ### use Reusable Workflows to call my workflow remotely
    ### https://docs.github.com/en/actions/learn-github-actions/reusing-workflows
    ### you can also call workflows from inside the same repo via file path

    # FIXME: customize uri to point to your own reusable linter repository
    uses: equinix-labs/super-linter-workflow/.github/workflows/reusable-super-linter.yaml@main

    ### Optional settings examples

    # with:
    ### For a DevOps-focused repository. Prevents some code-language linters from running
    ### defaults to false
    ### devops-only: false

    ### A regex to exclude files from linting
    ### defaults to empty
    ### filter-regex-exclude: html/.*

    ### If you're using the 'prettier' javascript style with eslint linting set this to true
    ### defaults to false
    ### javascript-use-prettier: true
```

## How to run Super-Linter locally

Option 1: Use [nektos/act](https://github.com/nektos/act) to run an existing GitHub Action workflow on your local repository clone.

Option 2: Use Docker to [run the Super-Linter image directly](https://github.com/github/super-linter/blob/main/docs/run-linter-locally.md).

Option 3: Pick the linter you want to run from Super-Linter, then install it locally to run manually. If you have a linter config, be sure to point the linter to `.github/linters/*`, and also realize that super-linter has default linter configs, that may change the linters behavior inside super-linter, with [templates listed here](https://github.com/github/super-linter/tree/main/TEMPLATES).

## Support

This repository is [Experimental](https://github.com/equinix-labs/equinix-labs/blob/main/experimental-statement.md) meaning that it's based on untested ideas or techniques and not yet established or finalized or involves a radically new and innovative style! This means that support is best effort (at best!) and we strongly encourage you to NOT use this in production.
