# [pre-commit.yml](pre-commit.yml)

## Description
This GitHub Actions workflow executes [pre-commit](https://pre-commit.com/) run between the base ref and the head ref of a Pull Request.

## Usage
```yaml
name: Validate PR
run-name: "Validate PR: ${{ github.head_ref }} ➡️ ${{ github.base_ref }} by @${{ github.actor }} (#${{ github.run_attempt }})"

concurrency:
  # pushes to the same branch will cancel the previous run
  group: ${{ github.workflow }}-${{ github.head_ref }}
  cancel-in-progress: true

on:
  # Note: pull_request_target is used to run the workflow on the base branch (main) instead of the head branch.
  pull_request_target:
    branches: ["main"]

  # Uncomment the following lines to run the workflow on the head branch, for example, to test the workflow.
  # Tip: Create a temporary base branch to test the workflow. (e.g. base-test, from main)
  # pull_request:
  #   branches: ["base-test"]

permissions:
  contents: read # Required to checkout the repository
  pull-requests: write # Required to post a comment on the PR

jobs:
  pre-commit:
    runs-on: ubuntu-latest
    steps:
      # install tools required for your pre-commit hooks
      # ...

      # install pre-commit and run the hooks
      - name: ☑️ PR Validation
        uses: sorinlg/actions/pre-commit@main
```
