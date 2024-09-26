## Creating releases

For creating a branch for version x.y.z

    git checkout develop
    git pull
    git checkout -b release/x.y.z
    ## TODO: Bump version at this point on the *release* branch (see below for instructions)

    ## Get the version commit to develop
    git checkout develop
    git merge --no-ff release/x.y.z
    ## TODO: Bump next development version on the *develop* branch (see below for instructions)
    ## Checkout to the release branch
    git checkout release/x.y.z

Merging pull requests to release

    git pull https://github.com/zakarfin/oskari-frontend.git some-bugfix
    ## git cherry-pick from develop etc
    git push origin release/x.y.z

Merging changes back to master

    git pull
    git checkout master
    git pull
    git merge --no-ff release/x.y.z
    git tag -a x.y.z -m "Release x.y.z"
    git push origin master
    git push origin --tags

Merging changes back to develop

    git checkout develop
    git pull
    git merge --no-ff release/x.y.z
    ## possible merging of conflicts
    git push origin develop

Cleanup

    ## remove local branch
    git branch -D release/x.y.z
    ## remove remote branch
    git push origin :release/x.y.z

### Versioning the code
- Releases should move the minor version 1.0.0 -> 1.1.0
- Hotfixes should move the patch version so 1.0.0 -> 1.0.1

#### Server
    ## Checkout to branch that should have the version updated
    git checkout {branch}

    ## Run the maven versions plugin to update version
    mvn -N versions:set -DnewVersion=x.y.z

    ## Commit the changes to Git
    git add .
    git commit -m 'Bump version'
    git push

Develop branch version should always be the next version + "-SNAPSHOT". For an example if the version in master is "1.0.0", develop should be "1.1.0-SNAPSHOT".

#### Frontend

    ## checkout to branch that should have the version updated
    git checkout {branch}

    ## Edit the version number on package.json
    nano package.json

    ## Commit the changes to Git
    git add .
    git commit -m 'Bump version'
    git push

#### Creating hotfixes
Much like creating releases except hotfixes are based on the master version (releases are based on develop).

For creating a branch for version x.y.z

    git checkout master
    git pull
    git checkout -b hotfix/x.y.z

Merging pull requests to hotfix

    ## TODO: Bump version at this point (see below for instructions)
    git pull https://github.com/zakarfin/oskari-frontend.git hotfix/my-urgent-fix
    ## git cherry-pick from develop etc
    git push origin hotfix/x.y.z

Merging changes back to master

    git pull
    git checkout master
    git pull
    git merge --no-ff hotfix/x.y.z
    git tag -a x.y.z -m "Hotfix x.y.z"
    git push origin master
    git push origin --tags

Merging changes back to develop

    git checkout develop
    git pull
    git merge --no-ff hotfix/x.y.z
    ## possible merging of conflicts
    git push origin develop

Cleanup

    ## remove local branch
    git branch -D hotfix/x.y.z
    ## remove remote branch
    git push origin :hotfix/x.y.z
