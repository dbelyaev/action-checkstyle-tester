# action-checkstyle-tester

[![Checkstyle](https://github.com/dbelyaev/action-checkstyle-tester/actions/workflows/checkstyle.yml/badge.svg)](https://github.com/dbelyaev/action-checkstyle-tester/actions/workflows/checkstyle.yml)

A demonstration repository showing how to use the [dbelyaev/action-checkstyle](https://github.com/dbelyaev/action-checkstyle) GitHub Action with different input parameters. Use this repo to evaluate how Checkstyle violations appear in pull request reviews before adopting the action in your own projects.

## Workflow Configuration

The repository contains a single workflow at `.github/workflows/checkstyle.yml` that runs Checkstyle on every pull request:

```yaml
name: 'Checkstyle'

on: [pull_request, workflow_dispatch]

permissions: {}

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  checkstyle:
    name: runner / checkstyle
    runs-on: ubuntu-latest
    timeout-minutes: 5

    permissions:
      contents: read
      pull-requests: write

    steps:
      - uses: actions/checkout@df4cb1c069e1874edd31b4311f1884172cec0e10 # v6.0.3

      - uses: actions/setup-java@be666c2fcd27ec809703dec50e508c2fdc7f6654 # v5.2.0
        with:
          distribution: 'temurin'
          java-version: '11'

      - uses: dbelyaev/action-checkstyle@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          reporter: github-pr-review
```

The workflow uses the `github-pr-review` reporter, which posts Checkstyle violations as inline PR review comments.

## Example Pull Requests

See these PRs for live examples of how the action reports violations:

| PR | Demonstrates |
|----|--------------|
| [#9](https://github.com/dbelyaev/action-checkstyle-tester/pull/9) | Basic Checkstyle violations with Google conventions |
| [#10](https://github.com/dbelyaev/action-checkstyle-tester/pull/10) | Sun code conventions configuration |
| [#11](https://github.com/dbelyaev/action-checkstyle-tester/pull/11) | Properties file usage with custom configuration |

## Usage

To use this repository for your own testing:

1. [Fork this repository](https://github.com/dbelyaev/action-checkstyle-tester/fork)
2. Create a branch with Java files containing style violations
3. Open a pull request to see Checkstyle comments in action

You can modify the workflow to test different [input parameters](https://github.com/dbelyaev/action-checkstyle#input-parameters) such as `checkstyle_config`, `checkstyle_version`, or `filter_mode`.

## License

This project is licensed under the [MIT License](LICENSE).
