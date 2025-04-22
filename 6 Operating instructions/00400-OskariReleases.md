## Creating releases

Assumes:
```sh
git remote add origin https://github.com/someuser/oskari-frontend.git
git remote add upstream https://github.com/oskariorg/oskari-frontend.git
```
You can check your repository names and URLs with `git remote -v`.

### creating a branch for version `x.y.z`

```sh
git checkout develop
git pull upstream develop
git checkout -b release/x.y.z
```

Bump version on the `release` branch at this point (see below for instructions) and push it to GitHub:

```sh
git push upstream release/x.y.z
```

Get the version commit to develop
```sh
git checkout develop
git merge --no-ff release/x.y.z
```
Bump next development version on the `develop` branch (see below for instructions).

#### Merging pull requests to release branch

```sh
git checkout release/x.y.z
git pull https://github.com/someuser/oskari-frontend.git some-bugfix-branch
## git cherry-pick from develop etc
git push upstream release/x.y.z
```

#### Merging release to `master` branch

Ensure you have the latest codes for the `release` and `master` branches, then merge the release to master and tag it with the version.

```sh
git checkout release/x.y.z
git pull upstream release/x.y.z
git checkout master
git pull upstream master
git merge --no-ff release/x.y.z
# Tagging and pushing to remote
git tag -a x.y.z -m "Release x.y.z"
git push upstream master
git push upstream --tags
```

#### Merging changes back to `develop` branch from `master` branch

```sh
git checkout develop
git pull upstream develop
git merge --no-ff master
## possible merging of conflicts
git push upstream develop
```

#### Cleanup

```sh
## remove local branch
git branch -D release/x.y.z
## remove remote branch
git push upstream :release/x.y.z
```

### Versioning the code

- Releases should move the minor version 1.0.0 -> 1.1.0
- Hotfixes should move the patch version so 1.0.0 -> 1.0.1

#### Server

```sh
## Checkout to branch that should have the version updated
git checkout {branch}

## Run the maven versions plugin to update version
mvn -N versions:set -DnewVersion=x.y.z

## Commit the changes to Git
git add .
git commit -m 'Bump version'
git push
```
Develop branch version should always be the **next version + "-SNAPSHOT"**. For an example if the version in master is `1.0.0`, develop should be `1.1.0-SNAPSHOT`.

#### Frontend

```sh
## checkout to branch that should have the version updated
git checkout {branch}

## Edit the version number on package.json
nano package.json

## Commit the changes to Git
git add .
git commit -m 'Bump version'
git push
```

#### Creating hotfixes

Much like creating releases except hotfixes are based on the master version (releases are based on develop).

For creating a branch for version x.y.z

```sh
git checkout master
git pull upstream master
git checkout -b hotfix/x.y.z
```

Merging pull requests to hotfix

```sh
## TODO: Bump version at this point (see below for instructions)
git pull https://github.com/someuser/oskari-frontend.git hotfix/my-urgent-fix
## git cherry-pick from develop etc
git push upstream hotfix/x.y.z
```

Merging changes back to master

```sh
git pull upstream hotfix/x.y.z
git checkout master
git pull upstream master
git merge --no-ff hotfix/x.y.z
# Tagging and pushing to remote
git tag -a x.y.z -m "Hotfix x.y.z"
git push upstream master
git push upstream --tags
```

Merging changes back to develop

```sh
git checkout develop
git pull upstream develop
git merge --no-ff hotfix/x.y.z
## possible merging of conflicts
git push upstream develop
```
Cleanup

```sh
## remove local branch
git branch -D hotfix/x.y.z
## remove remote branch
git push upstream :hotfix/x.y.z
```
