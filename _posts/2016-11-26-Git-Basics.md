---
layout: post
title: Git Basics
date: 2016-11-26 17:49:19 +0800
---

### 与其他版本控制系统的区别

直接记录快照，而非差异比较
CVS SVN等记录文件差异
![](/assets/images/Git-file-diff.png)
Git记录文件快照
![](/assets/images/Git-file-snapshot.png)

### 文件三种状态
![](/assets/images/Git-local-operations.png)

### Main components of a Git repository
![](/assets/images/git/main-components.svg)

### Commit-level Operation
#### Reset

For example, the following command moves the hotfix branch backwards by two commits.
For example, the following command moves the hotfix branch backwards by two commits.

```powershell
git checkout hotfix
git reset HEAD~2
```
![](/assets/images/git/reset-example.svg)

In addition to moving the current branch, you can also get git reset to alter the staged snapshot and/or the working directory by passing it one of the following flags:

- --soft – The staged snapshot and working directory are not altered in any way.
- --mixed – The staged snapshot is updated to match the specified commit, but the working directory is not affected. This is the default option.
- --hard – The staged snapshot and the working directory are both updated to match the specified commit.

![](/assets/images/git/reset-mode.svg)

#### Checkout

```powershell
git checkout hotfix
```

![](/assets/images/git/checkout-example1.svg)

```powershell
git checkout HEAD~2
```

![](/assets/images/git/checkout-example2.svg)

#### Revert

Reverting undoes a commit by creating a new commit. This is a safe way to undo changes, as it has no chance of re-writing the commit history. For example, the following command will figure out the changes contained in the 2nd to last commit, create a new commit undoing those changes, and tack the new commit onto the existing project.

```powershell
git checkout hotfix
git revert HEAD~2
```

![](/assets/images/git/revert-example.svg)

### File-level Operations

#### Reset

```powershell
git reset HEAD~2 foo.py
```

![](/assets/images/git/reset-file-example.svg)

#### Checkout

```powershell
git checkout HEAD~2 foo.py
```

![](/assets/images/git/checkout-file-example.svg)

### Summary

| Command     | Scope           | Common use cases                                                     |
| -------     | -----           | -----------------                                                    |
|git reset	  | Commit-level	| Discard commits in a private branch or throw away uncommited changes |
|git reset	  | File-level	    | Unstage a file                                                       |
|git checkout | Commit-level	| Switch between branches or inspect old snapshots                     |
|git checkout |	File-level	    | Discard changes in the working directory                             |
|git revert	  | Commit-level	| Undo commits in a public branch                                      |
|git revert	  | File-level	    | (N/A)                                                                |
