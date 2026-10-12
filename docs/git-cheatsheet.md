# Git cheat sheet

Commands for everyday use. `<something>` means you replace it including the brackets, e.g. `git add <file>` → `git add Vault.sol`.

For the concepts see [`git-basics.md`](git-basics.md), for our workflow see [`../CONTRIBUTING.md`](../CONTRIBUTING.md).

---

## Our workflow in 10 lines

```bash
git switch dev                            # go to dev
git pull                                  # get latest changes
git switch -c feature/<task>              # new branch for your task
# ... edit files ...
git add .                                 # stage all changes
git commit -m "Describe what you did"     # save snapshot
git push -u origin feature/<task>         # upload (first time; afterwards just: git push)
# → open Pull Request on GitHub: base dev ← your branch
# → after merge:
git switch dev && git pull && git branch -d feature/<task>
```

---

## Setup (once)

| Command | Does |
|---|---|
| `git config --global user.name "Name"` | Set your name for commits |
| `git config --global user.email "mail"` | Set your email for commits |
| `git clone <url>` | Download a repo |

## Check what's going on

| Command | Does |
|---|---|
| `git status` | **The most important command.** Shows current branch, changed files, staged files. Use it constantly. |
| `git diff` | Shows changes you haven't staged yet |
| `git diff --staged` | Shows changes you've staged (what will be committed) |
| `git log --oneline` | Short list of past commits |
| `git log --oneline --graph --all` | Commit history of all branches as a graph |

Exit `git log` or `git diff` by pressing `q`.

## Saving changes

| Command | Does |
|---|---|
| `git add <file>` | Stage one file |
| `git add .` | Stage all changes in the current folder |
| `git commit -m "message"` | Commit the staged changes |

## Branches

| Command | Does |
|---|---|
| `git branch` | List local branches (`*` = current one) |
| `git branch -a` | List all branches incl. the ones on GitHub |
| `git switch <branch>` | Go to an existing branch |
| `git switch -c <new-branch>` | Create a new branch and go to it |
| `git branch -d <branch>` | Delete a local branch (only works if it's merged) |

You might see `git checkout <branch>` in tutorials. That's the older version of `git switch`, does the same.

## Syncing with GitHub

| Command | Does |
|---|---|
| `git pull` | Download and merge new commits for the current branch |
| `git push` | Upload your commits |
| `git push -u origin <branch>` | Upload a new branch for the first time |
| `git pull origin dev` | Merge the latest `dev` into your current branch |
| `git fetch --prune` | Update the info about GitHub branches, remove deleted ones from the list |

## Putting work aside

| Command | Does |
|---|---|
| `git stash` | Temporarily put away uncommitted changes (e.g. to switch branches) |
| `git stash pop` | Bring them back |
| `git stash list` | Show stashed stuff |

## Undoing things

| Situation | Command |
|---|---|
| Discard changes to a file (not staged yet) | `git restore <file>` ⚠️ changes are gone |
| Unstage a file (keep the changes) | `git restore --staged <file>` |
| Fix the message of the last commit (**not pushed yet**) | `git commit --amend -m "new message"` |
| Forgot a file in the last commit (**not pushed yet**) | `git add <file>` then `git commit --amend --no-edit` |
| Undo the last commit, keep the changes (**not pushed yet**) | `git reset --soft HEAD~1` |
| Undo a commit that's **already pushed** | `git revert <commit-id>` (creates a new commit that reverses it) |
| Abort a merge with conflicts | `git merge --abort` |

The rule: anything that rewrites history (`--amend`, `reset`) is only okay for commits you **haven't pushed yet**. For pushed stuff, use `revert`.

---

## Never do this (unless you really know why)

| Command | Why not |
|---|---|
| `git push --force` | Overwrites the history on GitHub, can delete other people's work |
| `git reset --hard` | Deletes uncommitted changes, no way back |
| Commit directly on `main` or `dev` | Skips review, see [`CONTRIBUTING.md`](../CONTRIBUTING.md) |
| `git add .` without `git status` first | You might commit stuff you didn't want to (e.g. `.env`) |

---

## Common error messages

| Message | Meaning / fix |
|---|---|
| `fatal: not a git repository` | You're not in the project folder. `cd` into it. |
| `Your branch is behind 'origin/...'` | Others pushed new commits. Run `git pull`. |
| `! [rejected] ... (fetch first)` when pushing | GitHub has commits you don't have yet. `git pull` first, then `git push`. |
| `CONFLICT (content): Merge conflict in <file>` | Two people changed the same lines. See [merge conflicts](../CONTRIBUTING.md#merge-conflicts-dont-panic). |
| `error: Your local changes would be overwritten by checkout` | You have uncommitted changes. Commit them, or `git stash` before switching branches. |
| `The current branch ... has no upstream branch` | New branch, never pushed. Use `git push -u origin <branch>`. |
| `nothing to commit, working tree clean` | Not an error. Everything's committed. |
| Weird editor opens after `git pull` | It's Vim asking for a merge commit message. Type `:wq` + Enter. |
