# Git branching
* [Article](http://nvie.com/posts/a-successful-git-branching-model/)
* [Graphic](http://nvie.com/files/Git-branching-model.pdf)

## Create new repository on GitHub via CLI
0. Specify repository name
   ```bash
   repo="name-of-the-new-repo"
   ```
1. Create repo
   ```bash
   curl -u JonasRudolph https://api.github.com/user/repos -d "{ \"name\": \"${repo}\" }"
   ```

## Create feature
0. Set feature name
   ```bash
   featureToCreate='my-feature'
   ```
1. Execute  
    ```bash
    git checkout -b "${featureToCreate}" develop && git push --set-upstream origin "${featureToCreate}"
    ```

## Finish feature
0. Add changes to CHANGELOG.md
1. Set feature name
    * Current branch
        ```bash
        featureToFinish=$(git rev-parse --abbrev-ref HEAD)
        ```
    * Other branch
        ```bash
        featureToFinish='my-feature'
        ```
2. Merge feature into develop and push
    ```bash
    git checkout develop && git merge --no-ff "${featureToFinish}" && git push
    ```
3. Remove feature branch
    ```bash
    git push --no-verify origin --delete "${featureToFinish}" && git branch -d "${featureToFinish}"
    ```

## Create release
0. Set release number
    ```bash
    releaseToCreate='X.X.X'
    ```
1. Create release branch
    ```bash
    git checkout -b "release-${releaseToCreate}" develop
    ```
2. Set release number and date in CHANGELOG.md
3. Remove unused sections from CHANGELOG.md
4. Add a new [Unreleased] entry to the CHANGELOG.md
5. Set release number in other files (e.g. package.json, documentation, changescripts, plugin.xml)
6. (Optional) Create a build
6. Push changes made
    ```bash
    git add -A &&  git commit -m "Release ${releaseToCreate}" && git push --set-upstream origin "release-${releaseToCreate}"
    ```

## Finish release
0. Set release number
    ```bash
    releaseToFinish="${releaseToCreate}"
    ```
1. Merge release into master  
    ```bash
    git checkout master && git merge --no-ff "release-${releaseToFinish}"
    ```
2. Create tag an push
    ```bash
    git tag -a "${releaseToFinish}" -m "For changes see [${releaseToFinish}] section in CHANGELOG.md" && git push && git push --tags
    ```
3. Merge release into develop and push
    ```bash
    git checkout develop && git merge --no-ff "release-${releaseToFinish}" && git push
    ```
4. Remove release branch
    ```bash
    git push --no-verify origin --delete "release-${releaseToFinish}" && git branch -d "release-${releaseToFinish}"
    ```

## Untrack branches that where deleted on the remote
```bash
git fetch --all --prune
```

## List local branches with their remote copies
(Branches, that were deleted on remote, will be marked as `gone`)
```bash
git branch -vv
```
