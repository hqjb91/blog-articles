#### Trunk Based

We work with a single main branch and create feature branches off main

```
git checkout main
git checkout -b feature_1
... do some changes to the code
git add .
git commit -m "Changes to code"
```

We then push this new branch

```
git push origin feature_1
```

And then we create a PR against the main branch to merge into main and merge it after approval
Second user will then have to rebase the remote main onto his local main branch before pushing his code

```
git checkout main
git pull --rebase origin main
git checkout feature_2 // assume already created and changes made to it with stale main
git rebase main
git push origin feature_2
```

When we are ready to release, we cut a release from main

```
git checkout main
git pull --rebase origin main
git checkout -b release/1
git push origin release/1
git tag -a -m "Releasing version 1" v1
```

#### Git Flow

Instead of a single main branch, we have both main and develop

```
git checkout main
git checkout -b develop
git push origin develop
```

Each developer will then branch feature branches off main and when the features are completed they are merged back into develop

```
git checkout develop
git checkout -b feature_1
... do some changes to the code
git add .
git commit -m "Changes to code"
git checkout develop
git merge feature_1
```

Once ready for release, we fork a release branch off develop and once ready to deploy, the release branch is merged into main and tagged with a version number + merged back to develop

```
git checkout develop
git checkout -b release/1
...deploy release branch and post deployment checks
git checkout main
git merge release/1
```

Hotfix branches are used to quickly patch production release and are based on main instead of develop

```
git checkout main
git merge hotfix/1
git checkout develop
git merge hotfix/1
```