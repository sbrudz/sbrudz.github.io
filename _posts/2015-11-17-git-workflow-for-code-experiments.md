---
layout: post
title:  "A Git workflow for code experiments"
author: Steve Brudz
date:   2015-11-17
description: "Have you ever been developing a feature and hit a fork in the road where there are multiple ways to build it and you’re not sure which one is best?  Here's a git-based workflow that I use to help choose the best path."
---
![A fork in the road](/img/fork-in-the-road.webp)
*Photo by [i_yudai](https://www.flickr.com/photos/y_i/2330044065/)*

Have you ever been developing a feature and hit a fork in the road where there are multiple ways to build it and you’re not sure which one is best? When I hit this point in my work, I like to try a few experiments, to explore a little way down each of the paths to see which one is the most promising.

If you’re not careful, though, these experiments can quickly turn your code into a mess, as experiment 2 may require changes to the same parts of the code as experiment 1. Add in some undos and redos and suddenly neither experiment works and you’re not quite sure how to get back to experiment 1.

A good experimental workflow fulfills the following requirements:

1. Each experiment should be isolated so that changes in one don’t affect the other
2. It should be easy to switch from one experiment to another and back again
3. When I determine that an experiment is the best path, I should quickly be able to use some or all of that code to continue developing my feature.

Here is how I use [Git](https://git-scm.com/) to do this.

## Isolate Experiments
For each experiment I want to conduct, I create a new git branch and switch to using it:

```shell
git checkout -b experiment_1
```

This isolates the changes for the experiment. I then follow the normal process of adding and committing changes. When I want to try a new experiment, I commit any remaining changes as a work in progress (WIP) commit:

```shell
git add .  # to pick up all changed and new files
git commit -m WIP
```

A WIP commit is a single commit with a commit message of simply WIP, a convention that indicates the changes in this commit may or may not work and should never be merged into master. Later on, I’ll describe how to get rid of the WIP commit to keep your git history clean.

## Switch Between Experiments
I then switch back to the original branch and create another branch for the next experiment:

```shell
git checkout my_feature_branch
git checkout -b experiment_2
```

I conduct experiment 2 and commit the changes as shown above.

Using multiple branches, I can switch easily between the different experiments:

```shell
git checkout experiment_1
git checkout experiment_2
```

## Incorporate Experiment’s Results
Once I decide which path is best, I merge those changes back to my feature branch:

```shell
git checkout my_feature_branch
git merge experiment_1
```

If `my_feature_branch` now has a WIP commit added to it, I’ll get rid of the commit but keep the changes using:

```shell
git reset head^  # each ‘^’ you add will undo one more commit
```

The `reset` command undoes the latest commit on the current branch (aka HEAD) by moving the HEAD pointer back to a previous commit. The default option for reset is `--mixed`, which keeps the changes from the commit but unstages them. So if your WIP commit had changed the show.html.erb file and you ran `git reset head^`, then when you run `git status`, you’ll see something like:

```
On branch my_feature_branch
Changes not staged for commit:
  (use “git add <file>…” to update what will be committed)
  (use “git checkout — <file>…” to discard changes in working directory)
  
    modified: app/views/user/show.html.erb

no changes added to commit (use “git add” and/or “git commit -a”)
```

For an in depth explanation of the reset command, see this excellent [blog post](https://git-scm.com/blog/2011/07/11/reset.html). (Thanks to [Randall Reed](https://twitter.com/randallocalypse) for pointing me to it.)

Once the changes for my experiment are on the feature branch, I’m ready to continue development on the feature using what I learned from the experiment.

Using Git branches for experiments has other advantages, too, such as allowing you to [cherry pick](http://git-scm.com/docs/git-cherry-pick) changes from one experiment to another or sharing an experiment with someone else by [pushing the branch](http://git-scm.com/docs/git-push) to a remote git repository.

It’s a simple workflow, but using it with discipline has saved me a lot of aggravation.

*Special thanks to [Randall Reed](https://twitter.com/randallocalypse) and [Julie Kwok](https://github.com/kwokster10) for their feedback on this post!*

*Originally published on Medium [here](https://medium.com/defmethod-works/quick-tip-git-workflow-for-code-experiments-82af10b1c5c4).*