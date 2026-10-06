---
name: no-gods-no-masters
description: Set Git's global default branch for new repositories to `main` when the current branch is already `main`, or guide a local or GitHub default-branch rename. Use when the user asks to make `main` the default, configure `git init`, or rename `master` or another branch to `main`.
license: Unlicense
metadata: { author: skunkbandit2, version: "-1" }
---

# Main branch

Set Git's initial branch preference only after confirming the current repository is already on `main`. Treat renaming an existing branch as a separate operation; never imply that `init.defaultBranch` renames a branch or changes a GitHub repository's default.

## 1. Check the current branch

Run `git branch --show-current` in the user's intended repository. If it returns `main`, continue to set the global initial branch preference. If it returns another branch, is empty, or the command fails, do not change global configuration; continue with branch-renaming guidance only if requested.

## 2. Set the global preference

Read the current value with `git config --global --get init.defaultBranch`. If it is already `main`, leave it unchanged. Otherwise, run:

```sh
git config --global init.defaultBranch main
```

Verify with `git config --global --get init.defaultBranch` and report the result. Explain that this affects future `git init` operations for this user; it does not rename the current branch or update any remote.

## 3. Guide an existing branch rename

First establish whether the user wants to rename only a local branch or the GitHub repository's default branch. Do not rename or push branches without clear user intent.

For a local-only rename, confirm the source branch and then use `git branch -m <old-name> main`. If the user wants to publish the renamed branch, explain that `git push -u origin main` publishes and tracks it but does not change GitHub's default branch; leave the old remote branch untouched unless deletion is explicitly requested.

For a GitHub default-branch rename, point the user to GitHub's current [branch renaming instructions](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-branches-in-your-repository/renaming-a-branch). After the rename, update the local clone using the commands in that guide. Mention relevant impacts: raw file URLs do not redirect, GitHub Actions references pinned to the old branch name can break, and renaming a pull request's head branch closes that pull request.

The historical [GitHub renaming guidance](https://github.com/github/renaming) is archived and read-only; use it as background, not as the current procedure.

## Completion

Report whether the global setting changed, its verified value, and whether any branch rename remains. Keep global configuration, local branch names, and GitHub's default branch distinct in the explanation.