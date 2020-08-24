# Git Commands

## Scenario 1

You are about to commit a change, but you notice that someone has modified the same file, which makes you unable to proceed.   

Solution:
1. `git stash`
2. `git pull`
3. `git stash pop`

Explanation:  
   
`git stash` : stash your code locally, and return the project to the last commit. `git pull` : pull commits. `git stash pop` : auto-merge the conflicted files.
