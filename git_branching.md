# Git branching
* [Article](http://nvie.com/posts/a-successful-git-branching-model/)
* [Graphic](http://nvie.com/files/Git-branching-model.pdf)

## Create new branch on GitHub via CLI
```bash
repo="name-of-the-new-repo"
curl -u JonasRudolph https://api.github.com/user/repos -d '{ "name": "${repo}" }'
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
   ```bash
   featureToFinish='my-feature'
   ```
2. Merge feature into develop and push
   ```bash
   git checkout develop && git merge --no-ff "${featureToFinish}" && git push
   ```
3. Remove feature branch
   ```bash
   git push origin --delete "${featureToFinish}" && git branch -d "${featureToFinish}"
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
2. Set release number in CHANGELOG.md
3. Add a new [Unreleased] entry to the CHANGELOG.md
4. Set release number in other files (e.g. package.json, documentation, changescripts)
5. Push changes made
   ```bash
   git add -A &&  git commit -m "${releaseToCreate}" && git push --set-upstream origin "release-${releaseToCreate}"
   ```

## Finish release
0. Set release number
   ```bash
   releaseToFinish='X.X.X'
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
   git push origin --delete "release-${releaseToFinish}" && git branch -d "release-${releaseToFinish}"
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
