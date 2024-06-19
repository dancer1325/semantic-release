<h1 align="center" style="border-bottom: none;">📦🚀 semantic-release</h1>
<h3 align="center">Fully automated version management and package publishing</h3>
<p align="center">
  <a href="https://github.com/semantic-release/semantic-release/discussions">
    <img alt="Join the community on GitHub Discussions" src="https://img.shields.io/badge/Join%20the%20community-on%20GitHub%20Discussions-blue">
  </a>
  <a href="https://github.com/semantic-release/semantic-release/actions/workflows/test.yml">
    <img alt="Build states" src="https://github.com/semantic-release/semantic-release/actions/workflows/test.yml/badge.svg">
  </a>
  <a href="https://securityscorecards.dev/viewer/?uri=github.com/semantic-release/semantic-release">
    <img alt="OpenSSF Scorecard" src="https://api.securityscorecards.dev/projects/github.com/semantic-release/semantic-release/badge">
  </a>
  <a href="#badge">
    <img alt="semantic-release: angular" src="https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release">
  </a>
</p>
<p align="center">
  <a href="https://www.npmjs.com/package/semantic-release">
    <img alt="npm latest version" src="https://img.shields.io/npm/v/semantic-release/latest.svg">
  </a>
  <a href="https://www.npmjs.com/package/semantic-release">
    <img alt="npm next version" src="https://img.shields.io/npm/v/semantic-release/next.svg">
  </a>
  <a href="https://www.npmjs.com/package/semantic-release">
    <img alt="npm beta version" src="https://img.shields.io/npm/v/semantic-release/beta.svg">
  </a>
</p>

## Highlights

- Fully automated whole package release workflow
  - determine next version number
  - generate release notes
  - publish the package
- Enforce [Semantic Versioning](https://semver.org) specification
- New features and fixes are immediately available to users
- Notify maintainers and users of new releases
- Publish on different distribution channels (such as [npm dist-tags](https://docs.npmjs.com/cli/dist-tag)) based on git merges
- Integrate with your [continuous integration workflow](docs/recipes/release-workflow/README.md#ci-configurations)
- Avoid potential errors associated with manual releases
- Support any [package managers and languages](docs/recipes/release-workflow/README.md#package-managers-and-languages) via [plugins](docs/usage/plugins/plugins.md)
- Simple and reusable configuration via [shareable configurations](docs/usage/shareableConfigurations/shareable-configurations.md)
- Support for [npm package provenance](https://github.com/semantic-release/npm#npm-provenance) that promotes increased supply-chain security via signed attestations on GitHub Actions

## How does it work?

### Commit message format

* Based on commit messages  
  * determine the consumer impact of changes in the codebase
    * == determines the next [semantic version](https://semver.org) number
  * generates a changelog
  * publishes the release
* Commit messages conventions
  * by default, uses [Angular Commit Message Conventions](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-format)
  * can be adjusted by plugins
    * [@semantic-release/commit-analyzer](https://github.com/semantic-release/commit-analyzer#options)
    * [@semantic-release/release-notes-generator](https://github.com/semantic-release/release-notes-generator#options)
* Extra tools to enforce valid commit messages
  * [commitizen](https://github.com/commitizen/cz-cli)
  * [commitlint](https://github.com/conventional-changelog/commitlint)
#### Examples
* Let's use default configuration & certain commit message -> release types get:

| Commit message                                                                                                                                                                                   | Release type                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| `fix(pencil): stop graphite breaking when too much pressure applied`                                                                                                                             | ~~Patch~~ Fix Release                                                                                           |
| `feat(pencil): add 'graphiteWidth' option`                                                                                                                                                       | ~~Minor~~ Feature Release                                                                                       |
| `perf(pencil): remove graphiteWidth option`<br><br>`BREAKING CHANGE: The graphiteWidth option has been removed.`<br>`The default graphite width of 10mm is always used for performance reasons.` | ~~Major~~ Breaking Release <br /> (Note that the `BREAKING CHANGE: ` token must be in the footer of the commit) |

### Automation with CI
* Goal
  * **semantic-release** is desired to be executed
    * on the CI environment
      * == no human is directly involved 
    * after every successful build on the release branch

### Triggering a release
* Independently the git strategy used (https://www.atlassian.com/git/tutorials/comparing-workflows, [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow), ...) 
  * Based on the git webhooks set up (_Example:_ onPush, onMerge, ...) -> CI build is triggered
  * `semantic-release` command is triggered

* You can control the 
  * timing
    * _Example:_ [Maintenance releases](docs/recipes/release-workflow/maintenance-releases.md#publishing-maintenance-releases)
  * content
    * _Example:_ [Pre-releases](docs/recipes/release-workflow/pre-releases.md#publishing-pre-releases)
  * audience of published releases
    * _Example:_ [Using distribution channels](docs/recipes/release-workflow/distribution-channels.md#publishing-on-distribution-channels)

### Release steps
* each one -- is implemented by -- 👁️ configurable plugins 👁️
* How do they work?
  * once the tests are run -> `semantic-release` will execute the following steps ⚠️in this order ⚠️ 

| Step               | Required | Description                                                                                                                     |
|--------------------|----------|---------------------------------------------------------------------------------------------------------------------------------|
| `verifyConditions` | No       | Verify all the conditions to proceed with the release -- _Example:_ correct configuration, auth token are valid--               |
| Get last release   | ?        | Obtain the commit corresponding to the last release by analyzing [Git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging). |
| `analyzeCommits`   | Yes      | Based on the commits added since the last release - > determine the type of release                                             |
| `verifyRelease`    | No       | Verify the release conformity                                                                                                   |
| `generateNotes`    | No       | Generate release notes / commits added since the last release.                                                                  |
| Create Git tag     | ?        | Create a Git tag corresponding to the new release version.                                                                      |
| `prepare`          | No       | Prepare the release.                                                                                                            |
| `publish`          | No       | Publish the release.                                                                                                            |
| Notify             | ?        | Notify of new releases or errors.                                                                                               |
| `addChannel`       | No       | Add a release channel -- _Example:_ Add npm dist-tag --                                                                         |
| `success`          | No       | Notify a new release                                                                                                            |
| `fail`             | No       | Notify a failed release                                                                                                         |


## Requirements

In order to use **semantic-release** you need:

- To host your code in a [Git repository](https://git-scm.com)
- Use a CI service that allows you to [securely set up credentials](docs/usage/CI Configuration/ci-configuration.md#authentication)
- A Git CLI version that meets [our version requirement](docs/support/git-version.md) installed in your CI environment
- A [Node.js](https://nodejs.org) version that meets [our version requirement](docs/support/Node version/node-version.md) installed in your CI environment

## Documentation

- Usage
  - [Getting started](docs/usage/GettingStarted/getting-started.md)
  - [Installation](docs/usage/Installation/installation.md)
  - [CI Configuration](docs/usage/CI Configuration/ci-configuration.md)
  - [Configuration](docs/usage/Configuration/configuration.md#configuration)
  - [Plugins](docs/usage/plugins/plugins.md)
  - [Workflow configuration](docs/usage/workflow-configuration.md)
  - [Shareable configurations](docs/usage/shareableConfigurations/shareable-configurations.md)
- Extending
  - [Plugins](docs/extending/plugins/plugins-list.md)
  - [Shareable configuration](docs/extending/shareable-configurations-list.md)
- Recipes
  - [CI configurations](docs/recipes/ci-configurations/README.md)
  - [Git hosted services](docs/recipes/git-hosted-services/README.md)
  - [Release workflow](docs/recipes/release-workflow/README.md)
- Developer guide
  - [JavaScript API](docs/developer-guide/js-api.md)
  - [Plugins development](docs/developer-guide/plugin.md)
  - [Shareable configuration development](docs/developer-guide/shareable-configuration.md)
- Support
  - [Resources](docs/support/resources.md)
  - [Frequently Asked Questions](docs/support/FAQ.md)
  - [Troubleshooting](docs/support/troubleshooting.md)
  - [Node version requirement](docs/support/Node version/node-version.md)
  - [Node Support Policy](docs/support/node-support-policy.md)

## Get help

- [GitHub Discussions](https://github.com/semantic-release/semantic-release/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/semantic-release)
- [Twitter](https://twitter.com/SemanticRelease)

## Badge

Let people know that your package is published using **semantic-release** and which [commit-convention](#commit-message-format) is followed by including this badge in your readme.

[![semantic-release: angular](https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)

```md
[![semantic-release: angular](https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)
```

## Team

| [![Gregor Martynus](https://github.com/gr2m.png?size=100)](https://github.com/gr2m) | [![Pierre Vanduynslager](https://github.com/pvdlg.png?size=100)](https://github.com/pvdlg) | [![Matt Travi](https://github.com/travi.png?size=100)](https://github.com/travi) |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| [Gregor Martynus](https://github.com/gr2m)                                          | [Pierre Vanduynslager](https://github.com/pvdlg)                                           | [Matt Travi](https://github.com/travi)                                           |

## Alumni

| [![Stephan Bönnemann](https://github.com/boennemann.png?size=100)](https://github.com/boennemann) | [![Rolf Erik Lekang](https://github.com/relekang.png?size=100)](https://github.com/relekang) | [![Johannes Jörg Schmidt](https://github.com/jo.png?size=100)](https://github.com/jo) | [![Finn Pauls](https://github.com/finnp.png?size=100)](https://github.com/finnp) | [![Christoph Witzko](https://github.com/christophwitzko.png?size=100)](https://github.com/christophwitzko) |
| ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| [Stephan Bönnemann](https://github.com/boennemann)                                                | [Rolf Erik Lekang](https://github.com/relekang)                                              | [Johannes Jörg Schmidt](https://github.com/jo)                                        | [Finn Pauls](https://github.com/finnp)                                           | [Christoph Witzko](https://github.com/christophwitzko)                                                     |

<p align="center">
  <img alt="Kill all humans" src="media/bender.png">
</p>
