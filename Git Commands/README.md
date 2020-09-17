# Git Commands

## Scenario 1: Conflict

You are about to commit a change, but you notice that someone has modified the same file, which makes you unable to proceed.   

### Solution:

1. `git stash`: stash your code locally, and return the project to the last commit.
2. `git pull`: pull commits.
3. `git stash pop`: auto-merge the conflicted files.

## Scenario 2: Return back to a historical version

You have pushed commits to the remote branch but now you want to return back to a historical version.

### Solution:

1. `git reset --hard ` + commitId.
2. `git push -f -u origin master`.
