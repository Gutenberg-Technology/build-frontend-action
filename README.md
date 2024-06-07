# build-frontend-action

## Scenario

:warning: Requires a `.nvrmc` file in the root of the repository.

This Composite Action is a kind of wrapper for the following actions:

- Setup Node.js environment
- Optional pre-installation
- Install dependencies
- Lint code
- Build frontend
- Test
- Optionally, upload the archive to GitHub in order to use it later (only if `artifact-name` input is provided)

## Inputs

The Composite Action takes as inputs the `install`, `build`, `lint` and `test` commands to run and artifact upload options.

`cache-package-manager` input is used to manage cache in GitHub Actions, see [setup-node](https://github.com/actions/setup-node) and  [Advanced Usage](https://github.com/actions/setup-node/blob/main/docs/advanced-usage.md#caching-packages-data) for more details.

See the [action.yml](action.yml) file for more details.


## Basic examples

### Simple CI without lint

```yaml
name: CI

on:
  push:

jobs:
  ci_frontend:
    name: 'Build with development configuration'
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: Gutenberg-Technology/build-frontend-action@v1.0.0
        with:
          cache-package-manager: 'yarn'
          install-cmd: 'yarn install --immutable'
          build-cmd: 'yarn build:all'
          test-cmd: 'yarn test'
```

### With artifact upload

```yaml
name: build-and-upload

jobs:
  ci_frontend:
    name: 'Build with development configuration'
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: Gutenberg-Technology/build-frontend-action@v1.0.0
        with:
          cache-package-manager: 'yarn'
          install-cmd: 'yarn install --immutable'
          lint-cmd: 'yarn style'
          build-cmd: 'yarn build:all'
          test-cmd: 'yarn test'
          artifact-name: my-artifact-name
          artifact-path: dist/apps/
```
