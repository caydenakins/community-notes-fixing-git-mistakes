# Fixing Git Mistakes

Git will usually tells you what's wrong and how to fix it. Listen to the commands it gives you in the terminal/command line, it'll often solve your problems.

`git status` is very useful when you want to see the overall view/assess your situation.
- Always make sure if you have a modified file or untracked file to not make mistakes.

Once something is committed, deleting a branch doesn't get rid of it. We can even delete branches and recover the commits left under them if we haven't merged yet. Once we commit something it's in the commit tree and we can recover it.

Be very cautious using the force flag. `git push -f origin {branch-name}` could mess up a lot of things.

In order to prevent merge conflicts as a whole, make sure you pull before merging.

`git checkout {HEAD}` is detached head. It's dangerous because pointing to a specific commit, not something that automatically updates. If not careful, you can lose some changes.

## Useful Commands

`git remote -v` to see remotes.

`git status` to display state of working directory.

`git branch` is a pointer to a commit. HEAD can point to a branch or to a specific commit (see: detached HEAD).

`git checkout -b {branch-name}` to create new branches.

`git branch -d {branch-name}` deletes {branch-name}, but only locally because GitHub doesn't know we deleted the branch yet.
- To tell Git that: `git push --delete origin {branch-name}`.

`git branch -vv` lets you see branches being pointed to.

`git reflog -all` shows list of commits the HEAD has pointed to.

`git commit ---amend --no-edit` will amend a commit without changing the commit message.

`git remote prune origin` can be used to clean up local branches that cna be deleted/pruned.

`git rebase master` rewrites commit history, produces straight, linear commit history.

`git rebase -i {HEAD}`

## Different Stages of a File

### Working

Working directory is for making changes to files. Staging index, commonly messes up beginners. Add to the staging index, to prepare to commit to local repo you have (local repo/commit tree/HEAD).

### Stash

Think about it as a branch you can put stuff on. Use it when you try and pull something, but you have changes and there are conflicts, but want to separate those two. Stash changes, handle merge conflict, then pop your stash.

### Index

The position we place files in that we want committed to the repo.

### Remote

`git remote -v` see remotes: fetch and push. You are able to set up multiple remotes if you are pushing/pulling from multiple repos.

## Move files between the states (add/commit/push but move backwards checkout/reset/revert)

Adding files is done with `git add {file-name}` or `git add .` for all files.

To commit your files you have added, use `git commit -m "{message}"`.

When you are looking to undo commits, `git log` is going to be a valuable asset.

To show the Git diagram/flow, use `git log --oneline --graph`. Very useful when things get more complicated.

Types of resets in Git:

- Soft: takes it from commit tree/HEAD and moves it back to staging index. Can change message, add files, remove files.

- Mixed: moves it back to the working directory. It goes from commit tree/HEAD to working directory. (Back out of commit, but keep changes for a different time)

- Hard: goes back to working directory, vaporizes everything.

`git reset HEAD a.txt` becomes not staged for commit. Undoing an add is a reset to pull back to the working directory.

We can use `git reset {hash} --hard` to get rid of all work unless you committed it. This can be a dangerous approach and affect other team members.

We can use `git checkout {file-name}` to override our current local working directory with what's on the repo.

Using `git diff HEAD~1 HEAD` will diff the upper most change with the first up the tree.

To revert a commit, use `git revert {hash}`.

You don't want to git reset if you've already pushed. If you already pushed it, use `git revert {hash}`.

## Cherry Picking

Cherry pick does not move the commit from master to your other branch, it just puts it on top (still on master).

## Rebasing

If you are doing anything like amend/rebase, you should only be using it for commits you haven't pushed yet, else it can cause problems for other people.

### Interactive Rebase

`git rebase -i {HEAD}`

## Recover from bad states

`git log` will be a very useful command when you are dealing with recovery.

`git commit --amend {any flags}` gets last commit and change what you tell it to change. Amend is extremely helpful for changing the last commit.

We can compare differences between changes using `git diff {hash} {hash}`. To find hashes, use `git log --oneline`.

To revert a commit, use `git revert {hash}`.

## User questions

Anushree Subramani: *Do we need to use the set-upstream flag every time?*
- No, you only need to use it the first time. You can change this, but not recommended.

Anushree Subramani: *How to tackle it if someone has pulled the code from a branch but you end up force pushing a change to the same branch?*
- If there are no changes and they pull, they should be ok. If they did, they will have to make a new commit or reset.

Jean Kaplansky: *I get the point that you can push multiple amends to one commit, but should we assume this is not a best practice just because of the risks you take if the repo has multiple contributors?*
- Yes if you already pushed it. It's alright to rewrite your own commits because no one else has seen it, thus no downside because no one else would see it. If you push commits, I don't recommend actually changing it unless something is actually wrong. Actually wrong: seeker got leaked.

Matt Cowan: *do you need HEAD in that command?*
- Unneeded for the basic reset we covered, but later you will.

Anushree Subramani: *How to attach a detached HEAD back to its own branch?*
- On the commit you want to make a new branch, just checkout a new branch and you are able to do whatever you want with it.

Anushree Subramani: *What if I want to reattach it to its old branch?*
- Recommended to make a merge commit. See: cherry picking later.

Anushree Subramani: *Does everything get garbage collected in git reflog with time?*
- No it stays in commit history, only thing that gets reflog is orphaned things.

Cayden Akins: *Is it common to delete your branch after you get your work merged to master for the sake of organization?*
- Every team is different, but most commonly you will push your stuff to github, there is a code review, then the merger will delete your branch and you clean up other misc things afterwards.

Cayden Akins: *Can you reiterate how to decide between a rebase or a merge?*
- Rebase allows you to keep the straight history (no merges shown), whereas merging shows all the history. Instead of rebasing, which takes commits and stacks them on top of each other, merging takes commits and merges them together.
-
Pros of merging: Keep all history, aren't changing commits.

Cons of merging: Keeps all history and you have merge commits, which are extra commits.

Pros of rebasing: Nice, straight history.

Cons of rebasing: Lose all information of the merging and you may change information you pushed.

Typically, you will have your team pick between merging or rebasing and stick with it for projects.  

Raghu Bandaru: *Can you go through fixup vs squash*
- Squash will combine messages and fixup keeps the original, but discards the message from the fixup commit. Squash can change the commit message based on all the messages.
