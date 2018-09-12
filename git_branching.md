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
0. Execute  
    ```bash
    git_create_branch 'my-feature'
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
2. Merge feature into development and push
    ```bash
    git checkout development && git merge --no-ff --no-edit "${featureToFinish}" && git push
    ```
    * Conflict?
        1. Resolve conflict
        2. Commit and push
            ```bash
            git commit --no-edit && git push
            ```
        
      
3. Remove feature branch
    ```bash
    git_delete_branch_also_on_remote "${featureToFinish}"
    ```

## Create release
0. Set release number
    ```bash
    releaseToCreate='X.X.X'
    ```
1. Set release prefix
    ```bash
    releaseBranchPrefix='release'
    ```
2. Create release branch
    ```bash
    git checkout -b "${releaseBranchPrefix}-${releaseToCreate}" development
    ```
3. Set release number and date in CHANGELOG.md
4. Remove unused sections from CHANGELOG.md
5. Add a new [Unreleased] entry to the CHANGELOG.md
6. Set release number in other files (e.g. package.json, documentation, changescripts, plugin.xml, pom.xml)
7. Push changes made
    ```bash
    git add -A &&  git commit -m "Release ${releaseToCreate}" && git push --set-upstream origin "$(jr_funct_git_get_current_branch)"
    ```
8. (Optional) Create a build

## Finish release
0. Set release number
    ```bash
    releaseToFinish="${releaseToCreate}"
    ```
1. Set release prefix
    ```bash
    releaseBranchPrefix='release'
    ```
2. Merge release into master  
    ```bash
    git checkout master && git merge --no-ff --no-edit "${releaseBranchPrefix}-${releaseToFinish}"
    ```
3. Create tag an push
    ```bash
    git tag -a "${releaseToFinish}" -m "For changes see [${releaseToFinish}] section in CHANGELOG.md" && git push --no-verify && git push --no-verify --tags
    ```
4. Merge release into development and push
    ```bash
    git checkout development && git merge --no-ff --no-edit "${releaseBranchPrefix}-${releaseToFinish}" && git push --no-verify
    ```
    * Conflict?
        1. Resolve conflict
        2. Commit and push
            ```bash
            git commit --no-edit && git push
            ```
5. Remove release branch
    ```bash
    git_delete_branch_also_on_remote "${releaseBranchPrefix}-${releaseToFinish}"
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
