# Git branching
* [Article](http://nvie.com/posts/a-successful-git-branching-model/)
* [Graphic](http://nvie.com/files/Git-branching-model.pdf)

## Create feature
```bash
git checkout -b myfeature develop
git push --set-upstream origin myfeature
```

## Finish feature
1. Add changes to CHANGELOG.md
2. Execute
   ```bash
   # Merge feature into develop
   git checkout develop
   git merge --no-ff myfeature
   
   # Push develop branch
   git push
   
   # Remove feature branch
   git push origin --delete myfeature
   git branch -d myfeature
   ```

## Create release
1. Execute  
   ```bash
   git checkout -b release-X.X.X develop
   ```
2. Set release number in CHANGELOG.md
3. Set release number in other files (e.g. package.json)
4. Execute  
   ```bash
   git push --set-upstream origin release-X.X.X
   ```

## Commit release changes
```bash
git commit -m "Release X.X.X"
git push
```

## Finish release
```bash
# Merge release into master, create tag and push
git checkout master
git merge --no-ff release-X.X.X
git tag -a X.X.X
git push
git push --tags

# Merge release into develop and push
git checkout develop
git merge --no-ff release-X.X.X
git push

# Remove release branch
git push origin --delete release-X.X.X
git branch -d release-X.X.X
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
