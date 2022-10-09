# Misc commands

For git autocompletion follow [this](https://stackoverflow.com/a/19876372/7908040)

```text
curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash

# Add this to ~/.bashrc

if [ -f ~/.git-completion.bash ]; then
  . ~/.git-completion.bash
fi
```

List git config with `git config --list`

Check if file is tracked in staging area with `git status`

View log of another branch `git log branchname`

Files in **.gitignore** are not tracked, alternatively edit **.git/info/exclude** to ignore files on local site since .gitignore changes are tracked.

View differences in branches with `git diff branch1 branch2`

# Branches

Include this in ~/.bashrc to show your current branch in shell

```bash
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
export PAGER=less
export MANPAGER=less
export PS1="\[\e[32m\]\u \[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "
```

Your shell will look like

```bash
sarah (feature/checkout)$ 
```

Branches are pointers to a specific commit.

HEAD always points to where you currently are.

Create new branch with `git branch story/frogs-and-ox`, switch to it with `git switch branchname` and verify with `git branch`

See branch history including where it was checked out from with `git log --graph`

While on master, merge branch into master with `git merge story/frogs-and-ox`

[Explanation of what is git rebase](https://www.reddit.com/r/learnprogramming/comments/4ykgu4/eli5_what_is_git_rebase_and_common_use_cases_for/) and bonus [demo tutorial](https://learngitbranching.js.org/)

View all branches (local and remote) `git branch -a`

Can also view log of remote repo with `git log remotes/origin/master`

# Remote repositories

Note: **origin** is [an alias](https://stackoverflow.com/questions/9529497/what-is-origin-in-git) that refers to the remote repo where it was clone from, you can instead specify the repo URL `git push git@github.com:git/git.git master`

Link remote repository with local repo ` git remote add origin http://git.example.com/sarah/story-blog.git`

Push data to remote repo `git push origin branchname`

Setting a user as review isn't necessary for merge, just makes them party to comment on the changes.

## Fetch and pull

Fetch commit history from remote repo but not merge into local repo `git fetch origin master`

Merge changes from remote repo into local repo 

1. View all branches with `git branch -a`
2. Switch with `git branch`
3. Merge with `git merge remote-branch-from-1`

Or use `git pull repo branchname` which combines git fetch and merge

## Merge conflicts

Git conflict markers [[ref](https://stackoverflow.com/questions/7901864/git-conflict-markers)]

```text
<<<<<<< HEAD
1. The Lion and the Mooose
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
=======
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
>>>>>>> d30b24d51ae066b89ef11b3293827e7cb8e51b20
```

Top refers to local repo branch, bottom is version at remote repo with commit SHA1 hash of latest version.

Resolve conflict with

1. Edit file to final intended version
2. `git add` and `git commit`
3. `git push repo branchname`

Or alternatively abort merge with `git merge --abort`

## Rebasing

Rebasing will rewrite your branch history as a branch of the latest main branch, not an older one.

Steps:

1. Switch to branch with `git switch branchname`
2. Rebase your branch on top of master with `git rebase master`

Squash commits (combine multiple commits into 1):

1. Run `git rebase -i HEAD~3`
2. Edit the commit lines to `squash` instead of `pick`, or `fixup` if you want to discard each commit msg
   
   ```text
   pick f9791a0 Add first draft of hare-and-tortoise story
   squash 0387a8d Update hare-and-tortoise
   squash 4eb316b Finish hare-and-tortoise
   ```

# Cherry picking

Steps:

1. Find hash of target commit
2. Switch to branch and do `git cherry-pick SHA1`

# Reset and revert

Revert deletes changes made by the commit, and keep the undo revision itself in commit history. `git revert commithash`

## Hard vs soft reset

Retain changes made but not commits, do soft reset `git reset --soft HEAD~1`, check with `git status` that changes are not yet to be committed.

Using **hard** instead of **soft** will remove both commit from history and its changes altogether.

If you made changes without adding to staging, revert with `git restore .` or `git restore folder/specific/file`

# Stashing

The idea is to stash changes without committing them (into history) into some working directory which can be retrieved.

When you make changes without committing and you switch branch it shows this.

```text
error: Your local changes to the following files would be overwritten by checkout:
        README.md
Please commit your changes or stash them before you switch branches.
Aborting
```

Stash with `git stash`

List stash changes with `git stash list`, add `-p` to show all details

Show stash changes with `git stash show stash@{1}`

Retrieve from stash with `git stash pop stash@{0}`

# Reflog

To undo hard reset, use reflog. This shows the state of local repo for [90 days](https://stackoverflow.com/questions/17857723/whats-the-difference-between-git-reflog-and-log), plus all the actions and commits while log just shows commits.

Show local history with `git reflog`

Reset to commit with `git reset --hard commithash`
