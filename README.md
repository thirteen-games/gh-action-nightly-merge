# Nightly Merge Action

Automatically merge the stable branch into the development one.

If the merge is not necessary, the action will do nothing.
If the merge fails due to conflicts, the action will fail, and the repository
maintainer should perform the merge manually.

## Installation

To enable the action simply create the `.github/workflows/nightly-merge.yml`
file with the following content:

```yml
name: 'Nightly Merge'

on:
  schedule:
    - cron:  '*/0 0 * * *'

jobs:
  nightly-merge:

    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v1

    - name: Nightly Merge
      uses: robotology/gh-action-nightly-merge@v1.0.1
      with:
        stable_branch: 'master'
        development_branch: 'devel'
        allow_ff: false
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Even though this action was created to run as a scheduled workflow,
[`on`](https://help.github.com/en/articles/workflow-syntax-for-github-actions#on)
can be replaced by any other trigger.
For example, this will run the action whenever something is pushed on the
`master` branch:

```yml
on:
  push:
    branches:
      - master
```

## Parameters

### `stable_branch`

The name of the stable branch (default `master`).

### `development_branch`

The name of the development branch (default `devel`).

### `allow_ff`

Allow fast forward merge (default `false`). If not enabled, merges will use `--no-ff`.

### `allow_forks`

Allow action to run on forks (default `false`).
