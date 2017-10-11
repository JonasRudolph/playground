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
   featureName='my-feature'
   ```
1. Execute  
   ```bash
   git checkout -b "${featureName}" develop
   git push --set-upstream origin "${featureName}"
   ```

## Finish feature
0. Add changes to CHANGELOG.md
1. Set feature name
   ```bash
   featureName='my-feature'
   ```
2. Execute
   ```bash
   # Merge feature into develop
   git checkout develop
   git merge --no-ff "${featureName}"
   
   # Push develop branch
   git push
   
   # Remove feature branch
   git push origin --delete "${featureName}"
   git branch -d "${featureName}"
   ```

## Create release
0. Set release number
   ```bash
   releaseNumber='X.X.X'
   ```
1. Execute  
   ```bash
   git checkout -b "release-${releaseNumber}" develop
   ```
2. Set release number in CHANGELOG.md
3. Add a new [Unreleased] entry to the CHANGELOG.md
4. Set release number in other files (e.g. package.json, documentation, changescripts)
5. Execute
   ```bash
   git add CHANGELOG.md [package.json ...]
   git commit -m "${releaseNumber}"
   git push --set-upstream origin "release-${releaseNumber}"
   ```

## Finish release
0. Set release number
   ```bash
   releaseNumber='X.X.X'
   ```
1. Execute   
   ```bash
   # Merge release into master, create tag and push
   git checkout master
   git merge --no-ff "release-${releaseNumber}"
   git tag -a "${releaseNumber}" -m "For changes see [${releaseNumber}] section in CHANGELOG.md"
   git push
   git push --tags

   # Merge release into develop and push
   git checkout develop
   git merge --no-ff "release-${releaseNumber}"
   git push

   # Remove release branch
   git push origin --delete "release-${releaseNumber}"
   git branch -d "release-${releaseNumber}"
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
