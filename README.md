# Helm Publisher

A GitHub Action for publishing Helm charts with Github Pages.

## Usage

Inputs:

* `token` The GitHub token with write access to the target repository
* `charts_dir` The charts directory, defaults to `charts`
* `charts_url` The GitHub Pages URL, defaults to `https://<OWNER>.github.io/<REPOSITORY>`
* `owner` The GitHub user or org that owns this repository, defaults to the owner in `GITHUB_REPOSITORY` env var
* `repository` The GitHub repository, defaults to the `GITHUB_REPOSITORY` env var
* `branch` The branch to publish charts, defaults to `gh-pages`
* `target_dir` The target directory to store the charts, defaults to `.`
* `helm_version` The Helm CLI version, defaults to the latest release
* `linting` Toggle Helm linting, can be disabled by setting it to `off`
* `commit_username` Explicitly specify username for commit back, default to `GITHUB_ACTOR`
* `commit_email` Explicitly specify email for commit back, default to `GITHUB_ACTOR@users.noreply.<GITHUB_HOST>`

## History

this action is based on the work of those actions:

* https://github.com/stefanprodan/helm-gh-pages
* https://github.com/helm/chart-releaser-action

### downsides of helm/chart-releaser-action

Chart releaser action do push the charts to releases, which makes the action not usable for private repoistorys, where
you e.g want to use the repo from https://raw.github.com.
Additionally it does not work for github enterprise.

### downsides of stefanprodan/helm-gh-pages

The action does not work for github enterprise. And chart-releaser-action does not overwrite already released charts.


## Examples

Package and push all charts in `./charts` dir to `gh-pages` branch:

```yaml
name: release
on:
  push:
    tags: '*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Publish Helm charts
        uses: paschdan/helm-chart-release-to-repo-action@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
```
