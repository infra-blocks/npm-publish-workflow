# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.1] - 2024-05-02

### Fixed

- Use `inputs.skip` at step level, and not at job level. Skipping at job level results in the same issue with
  required checks paths as skipping at the caller's workflow invocation site (with an `if` expression).

## [1.2.0] - 2024-04-19

### Added

- The `skip` input to skip on the callee's side of a `worfklow_call`. Otherwise, when the workflow is skipped from
the caller's side (with an `if` expression), the check path when the workflow was run is not the same as when
it was skipped ( `outer / inner` vs. `outer` when skipped) !

## [1.1.1] - 2024-03-08

### Added

- Status badges in README.

### Fixed

- The link to the version produced

## [1.1.0] - 2024-01-27

### Added

- `git-tag` output.

## [1.0.4] - 2024-04-27

### Added

- A link to the version produced in the `links` output.

## [1.0.3] - 2024-01-27

### Fixed

- Broken workflow output typo.

## [1.0.2] - 2024-01-26

### Fixed

- Version shown in usage section of the README.

## [1.0.1] - 2024-01-24

### Fixed

- The CI invocation cycle caused by pushing the commit produced by `npm version`. Introduced a `skip-ci` optional
input.

## [1.0.0] - 2024-01-22

### Added

- First release!

[1.2.1]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.1.1...v1.2.0
[1.1.1]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.0.4...v1.1.0
[1.0.4]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.0.3...v1.0.4
[1.0.3]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.0.2...v1.0.3
[1.0.2]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/infra-blocks/npm-publish-workflow/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/infra-blocks/npm-publish-workflow/releases/tag/v1.0.0
