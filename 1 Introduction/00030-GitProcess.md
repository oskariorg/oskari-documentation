## Git process

This document describes the source management process used by the Oskari project. As internal development at NLS uses a ​rather ​well ​documented ​branching ​model called ​Git Flow, this document emphasizes interfacing with external developers/teams instead of reiterating ​Git Flow documentation readily available on the Web.

### Oskari Git Flow overview

![gitflow.svg](../resources/images/gitflow.svg)

Read Atlassian's awesome [Git Flow documentation](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) for more information.

### Branches & Repos

**master**
- The latest stable release.
- Tagged according to Oskari's versioning scheme.
- Commits merged from hotfix and release branches.

**develop**
- The work-in-progress next release.
- Commits merged from pull requests.

**release/x.y.z**
- A release branch is opened as a code freeze some time before the next release based on the current develop-branch.
- Release branch should only include bugfixes to the features that will be in the next release.
- Any new features should be merged to develop-branch for a release after the next one.
- Once the release testing has been completed the branch will be merged into develop and master.

**hotfix/x.y.z**
- When urgent bugfixes are required for the latest release a hotfix-branch is opened based on the current master-branch.
- Once the fixes are completed the branch will be merged into develop and master.

**{any-other-branches}**
- Sometimes opened for developing larger change sets that might require multiple pull requests of development before they can be merged to develop without breaking things unintentionally.
- Once the feature is completed the branch will be merged into develop
- Most functionality development should be done in forks, not the official repositories.
- See [how to contribute](../7 Operating instructions/00310-HowToContributeToOskariProject.md) for Git instructions

**External branches**

The branching model utilized by an external team is generally irrelevant to the process of handling external contributions. It is, however, assumed for the purposes of this document that there exists a continuous sequence of commits from the latest merge from Oskari branches on GitHub to the commit(s) presented for consideration for inclusion in Oskari proper. This is required in order to be able to rebase such commits on **the develop branch**.
