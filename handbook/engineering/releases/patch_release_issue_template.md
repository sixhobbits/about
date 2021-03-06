<!--
DO NOTE COPY THIS ISSUE TEMPLATE MANUALLY. Use `yarn run release tracking:patch-issue <version>` from the
`dev/release` directory in the main repository to create a patch release issue, instead.

Arguments:
- $MAJOR
- $MINOR
- $PATCH
-->

# $MAJOR.$MINOR.$PATCH Patch Release

**Attention developers:** to get your commits in `main` included in this patch release, please file a [patch request](https://github.com/sourcegraph/sourcegraph/issues/new?assignees=&labels=team%2Fdistribution&template=request_patch_release.md&title=$MAJOR.$MINOR.$PATCH%3A+) for review. Only check off items if the commit have been cherry-picked into the `$MAJOR.$MINOR` branch by the release captain.

- [ ] TODO: Add [patch request issues](https://github.com/sourcegraph/sourcegraph/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3Apatch-release-request) and their associated commits on `main` here

---

**Note:** All `yarn run release ...` commands should be run from folder `dev/release`.

**Note:** All `yarn run test ...` commands should be run from folder `web`.

## Setup

- [ ] Ensure release configuration in [`dev/release/release-config.jsonc`](https://sourcegraph.com/github.com/sourcegraph/sourcegraph/-/blob/dev/release/release-config.jsonc) on `main` is up to date with the parameters for the current release.
- [ ] Ensure you have the latest version of the release tooling and configuration by checking out and updating `sourcegraph@main`.

## Prepare release

Ensure all patch changes are included:

- [ ] Ensure that all [patch request issues](https://github.com/sourcegraph/sourcegraph/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3Apatch-release-request) are accounted for, and that the commits listed above has been cherry-picked to [`$MAJOR.$MINOR` release branch](https://github.com/sourcegraph/sourcegraph/tree/$MAJOR.$MINOR), and ensure CI passes on the release branch.
    ```
    git checkout $MAJOR.$MINOR
    git pull
    git cherry-pick <commit0> <commit1> ... # all relevant commits from the main branch
    git push $MAJOR.$MINOR
    ```
- [ ] Cherry-pick relevant [deploy-sourcegraph](https://github.com/sourcegraph/deploy-sourcegraph) configuration changes from `master` to the [`$MAJOR.$MINOR` release branch](https://github.com/sourcegraph/deploy-sourcegraph/tree/$MAJOR.$MINOR), and ensure CI passes on the release branch.
    ```
    git checkout $MAJOR.$MINOR
    git pull
    git cherry-pick <commit0> <commit1> ... # all relevant commits from the master branch
    git push $MAJOR.$MINOR
    ```
- [ ] Cherry-pick relevant [deploy-sourcegraph-docker](https://github.com/sourcegraph/deploy-sourcegraph-docker) configuration changes from `master` to the [`$MAJOR.$MINOR` release branch](https://github.com/sourcegraph/deploy-sourcegraph-docker/tree/$MAJOR.$MINOR), and ensure CI passes on the release branch.
    ```
    git checkout $MAJOR.$MINOR
    git pull
    git cherry-pick <commit0> <commit1> ... # all relevant commits from the master branch
    git push $MAJOR.$MINOR
    ```

Create and test release candidates:

- [ ] Push a release candidate tag:
    ```
    yarn run release release:create-candidate 1
    ```
- [ ] Ensure the [Sourcegraph pipeline](https://buildkite.com/sourcegraph/sourcegraph/builds?branch=$MAJOR.$MINOR), [QA pipeline](https://buildkite.com/sourcegraph/qa/builds?branch=$MAJOR.$MINOR), and [E2E pipeline](https://buildkite.com/sourcegraph/e2e/builds?branch=$MAJOR.$MINOR) in Buildkite passes.

## Stage release

- [ ] Tag the final release:
    ```sh
    yarn run release release:create-candidate final
    ```
- [ ] Ensure the [Sourcegraph pipeline](https://buildkite.com/sourcegraph/sourcegraph/builds?branch=$MAJOR.$MINOR), [QA pipeline](https://buildkite.com/sourcegraph/qa/builds?branch=$MAJOR.$MINOR), and [E2E pipeline](https://buildkite.com/sourcegraph/e2e/builds?branch=$MAJOR.$MINOR) in Buildkite passes.
- [ ] Open PRs that publish the new release and address any action items required to finalize draft PRs (track PR status via the [generated release campaign](https://k8s.sgdev.org/organizations/sourcegraph/campaigns)):
  ```sh
  yarn run release release:stage
  ```

## Release

<!-- Keep in sync with release_issue_template's "Release" section -->

- [ ] From the [release campaign](https://k8s.sgdev.org/organizations/sourcegraph/campaigns), merge the release-publishing PRs created previously.
  - For [sourcegraph](https://github.com/sourcegraph/sourcegraph), also:
    - [ ] Cherry pick the release-publishing PR from `sourcegraph/sourcegraph@main` into the release branch.
- [ ] Finalize and announce that the release is live:
  ```sh
  yarn run release release:close
  ```

## Post-release

- [ ] Open a PR to update [`dev/release/release-config.jsonc`](https://sourcegraph.com/github.com/sourcegraph/sourcegraph/-/blob/dev/release/release-config.jsonc) with the parameters for the current release.
- [ ] Ensure you have the latest version of the release tooling and configuration by checking out and updating `sourcegraph@main`.
- [ ] Close this issue.

**Note:** If another patch release is requested after the release, ask that a [patch request issue](https://github.com/sourcegraph/sourcegraph/issues/new?assignees=&labels=team%2Fdistribution&template=request_patch_release.md) be filled out and approved first.
