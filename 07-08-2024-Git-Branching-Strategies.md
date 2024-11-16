# Git Workflows: Trunk-Based and Git Flow

A comparison of two popular Git workflows, **Trunk-Based Development** and **Git Flow**, including their steps and usage scenarios.

---

## Trunk-Based Development

### Overview
Trunk-Based Development relies on a single main branch. Feature branches are created and merged into the main branch via pull requests. Rebasing is used to keep local branches up to date with the remote main branch.

### Workflow Steps

#### 1. Create and Work on a Feature Branch
```bash
git checkout main
git checkout -b feature_1
# Make changes to the code
git add .
git commit -m "Changes to code"
```

#### 2. Push the Feature Branch
```bash
git push origin feature_1
```

#### 3. Merge Feature Branch via Pull Request
- Create a pull request (PR) to merge `feature_1` into `main`.
- After approval, merge the PR.

#### 4. Rebase Main Before Working on Another Feature
```bash
git checkout main
git pull --rebase origin main
git checkout feature_2  # Assume the feature branch already exists
git rebase main
git push origin feature_2
```

#### 5. Create a Release Branch
```bash
git checkout main
git pull --rebase origin main
git checkout -b release/1
git push origin release/1
git tag -a -m "Releasing version 1" v1
```

---

## Git Flow

### Overview
Git Flow uses multiple branches, typically `main` and `develop`, to separate released code from code under active development. Features are merged into `develop`, and releases are branched off and merged back into `main` and `develop`.

### Workflow Steps

#### 1. Initialize `develop` Branch
```bash
git checkout main
git checkout -b develop
git push origin develop
```

#### 2. Create and Work on a Feature Branch
```bash
git checkout develop
git checkout -b feature_1
# Make changes to the code
git add .
git commit -m "Changes to code"
```

#### 3. Merge Feature Branch into `develop`
```bash
git checkout develop
git merge feature_1
```

#### 4. Create a Release Branch
```bash
git checkout develop
git checkout -b release/1
# Deploy the release branch and perform post-deployment checks
git checkout main
git merge release/1
```

#### 5. Handle Hotfixes
Hotfix branches are based on `main` and used for quick production patches. They are merged back into both `main` and `develop`.
```bash
git checkout main
git merge hotfix/1
git checkout develop
git merge hotfix/1
```

---

### Comparison of Workflows

| **Aspect**         | **Trunk-Based Development**           | **Git Flow**                      |
|---------------------|---------------------------------------|-----------------------------------|
| **Branches**        | Single `main` branch                 | Separate `main` and `develop`    |
| **Release Process** | Releases are directly cut from `main`| Releases are merged from `develop` into `main` |
| **Hotfix Handling** | Applied directly to `main`           | Hotfixes branch off `main` and merge into both `main` and `develop` |
