## This repository has been abandoned

GitHub now offers a much cleaner way to handle linters with complex projects and provide annotations on PRs. [The problem matcher format](https://github.com/actions/toolkit/blob/master/docs/problem-matchers.md) is now the best way to handle this.

[`actions/setup-node`](https://github.com/actions/setup-node) provides ESLint annotations by default now, and it is the best solution for lint annotations in PRs. See https://github.com/actions/setup-node/blob/main/.github/eslint-stylish.json as an example of the problem matcher format.

# ESLint Action (abandoned)

This is a GitHub Action that runs ESLint for `.js`, `.jsx`, `.ts` and `.tsx` files using your `.eslintrc` rules. It's free to run and it'll annotate the diffs of your pull requests with lint errors and warnings.

![](screenshots/annotation.png)

Neat! Bet your CI doesn't do that.

## Usage

`.github/workflows/lint.yml`:
```yml
name: Lint

on: pull_request

jobs:
  eslint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: hallee/eslint-action@1.0.3
        # GITHUB_TOKEN in forked repositories is read-only
        # https://help.github.com/en/actions/reference/events-that-trigger-workflows#pull-request-event-pull_request
        if: ${{ github.event_name == 'push' || github.event.pull_request.head.repo.full_name == github.repository }} 
        with:
          repo-token: ${{secrets.GITHUB_TOKEN}}
          source-root: optional-sub-dir
```
