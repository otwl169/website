---
title: "git cheatsheet"
date: 2023-06-26T16:51:36+01:00
---

While not comprehensive, this post will detail some common `git` commands which I either forget often or think are worth noting down somewhere easily accessible. The notes are written for people who have used `git` before.

## git reset
There are 3 modes to git reset: soft, mixed and hard.

 - `git reset --soft`: leaves local changes alone, detailing them as to be committed; resets head to commit.
 - `git reset`: mixed is the default mode; keeps changed files, but not added.
 - `git reset --hard`: discards and deletes any untracked files; resets local files and git tree to specified commit.

## git revert
Use this command when you've merged or pushed changes you want to undo to a branch that others are using. This is how to undo a change without rewriting history. 

If you were to affect the history, it would mess with everyone's local version of a branch. Everyone would need to rebase their feature branches on top of the new history before being able to merge.

## git merge vs git rebase
`git merge feature main` adds a commit to your feature branch incorporating changes from main.

`git rebase feature main` simply sets base of the feature branches commits to the top of main. In other words, it starts a branch with the name feature at the top of main and then replays each of your commits into this branch. ***NEVER*** run `git rebase main feature` as this will mess with the history of main, which is always a bad thing when working with others.

Merge and rebase conflicts are very similar and can be handled in the same way using your editor of choice (e.g [VScode merge conflicts](https://code.visualstudio.com/docs/sourcecontrol/overview#_merge-conflicts)).

Personally, I find working with `git rebase` to be a lot simpler.

## git rebase interactive mode
`git rebase -i <commit>` is one of the most useful git commands! It by default will open up your shell text editor. For example in a small git repository running `git rebase -i HEAD~2`:

![Screenshot of git rebase interactive](/git-rebase-interactive.png)

The top commit is 1 commit before `HEAD` (as we are rebasing onto a base 2 commits before). From here, we can do things such as combining two commits, changing a commit's message as well as stopping somewhere along the rebase to change a couple files before continuing along with `git rebase --continue`!

Note that if you also want to include the root commit in such a way, you can't as there is no parent. You can get around this by running `git rebase -i --root`.

## HEAD~ vs HEAD^
`HEAD~n` refers to `n` commits back. 

`HEAD^n` is used to distinguish between multiple parents of a commit (for example, a merge commit has the commit of the main and feature branch as parents).

Speaking of multiple parents of a commit, you can have as many as you want! Scarily you can run `git merge branch1 branch2 branch3 ...`, which is called an **octopus merge**. I am afraid of this technology and will not attempt it.

## git reflog
Accidentally `git reset --hard` to a previous commit, losing all of your hard work? No problem. Type `git reflog` to see logs for each git command you have run, and then find the reflog for the commit before you messed up. Then you can simply `git reset --HARD <sha>` to get right back to it. Note that for rebases, since each commit is 
