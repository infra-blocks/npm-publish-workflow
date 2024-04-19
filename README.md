# npm-publish-workflow
[![Git Tag Semver From Label](https://github.com/infrastructure-blocks/npm-publish-workflow/actions/workflows/git-tag-semver-from-label.yml/badge.svg)](https://github.com/infrastructure-blocks/npm-publish-workflow/actions/workflows/git-tag-semver-from-label.yml)
[![Update From Template](https://github.com/infrastructure-blocks/npm-publish-workflow/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infrastructure-blocks/npm-publish-workflow/actions/workflows/update-from-template.yml)

This reusable workflow runs all the necessary steps to publish a new version of an NPM package based on the
version provided.

It checks out the code, prepares the dependencies, runs `npm version`, `npm publish`, and finally pushes
the resulting commit and tag upstream.

Any version that is accepted by `npm version` can be provided as input.

If pushing against a protected branch, then a GitHub PAT should be provided. The user owning that PAT should have
permissions to bypass the protection branch. This way, we can use `git push` instead of `git push --force`, which
could have undesired consequences.

## Inputs

|   Name    | Required | Description                                                                                                                                                                                         |
|:---------:|:--------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  version  |   true   | The release type or the new release version. Either a semantic version or one of "major", "minor", "patch", "premajor", "preminor", "prepatch", or "prerelease".                                    |
| dist-tags |  false   | A stringified JSON array of the dist-tags to apply. Defaults to '["latest"]'                                                                                                                        |
|   skip    |  false   | A boolean indicating whether to skip whether to skip the workflow. This is to workaround the required checks path issue when workflows are skipped. It defaults to false.                           |
|  skip-ci  |  false   | A boolean indicating whether to skip the CI when pushing the git commit or not. This is especially useful if using this workflow on a push event with a GitHub PAT, for example. Defaults to false. |

## Secrets

|     Name     | Required | Description                                                                                                                                                                      |
|:------------:|:--------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| github-token |   true   | The GitHub token used to configure the Git CLI. It should have the rights to push code and tags. When the branch or the tags are protected, this should be a PAT.                |
|  npm-token   |   true   | The NPM token used to publish the package. This is passed as the NPM_TOKEN environment variable. See [ts-lib-template](https://github.com/infrastructure-blocks/ts-lib-template) |

## Outputs

|     Name     | Description                                                                                                                                                   |
|:------------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| package-name | The name of the processed package. Example: @infra-blocks/lib-template                                                                                        |
|   version    | The version of the released package. Example: 1.2.3-alpha.5                                                                                                   |
|  dist-tags   | The stringified JSON array of dist-tags applied. This will match the input.                                                                                   |
|    links     | A stringified JSON array of markdown links to the released package's registry versions of the form:<br/> ["\[<package-identifier>\](<version-registry-url>)"] |
|   git-tag    | The Git tag produced.                                                                                                                                         |

## Permissions

|  Scope   | Level | Reason                                                                                                     |
|:--------:|:-----:|------------------------------------------------------------------------------------------------------------|
| contents | write | In case the GitHub token provided is the GITHUB_TOKEN. This will be required to push the changes upstream. |

## Concurrency controls

N/A

## Timeouts

N/A

## Usage

```yaml
name: NPM Publish

on:
  push: ~

permissions:
  contents: write

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  npm-publish:
    uses: infrastructure-blocks/npm-publish-workflow/.github/workflows/workflow.yml@v1
    permissions:
      contents: write
    with:
      version: major
      dist-tags: '["latest", "git-sha-${{ github.event.pull_request.head.sha }}"]'
    secrets:
      github-token: ${{ secrets.PAT }}
      npm-token: ${{ secrets.NPM_PUBLISH_TOKEN }}

```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
