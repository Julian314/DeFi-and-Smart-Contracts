# How we work on this repo

This file explains our workflow, meaning how we use branches and how code gets from your laptop into the project.

If you've never used git before, read [`docs/git-basics.md`](docs/git-basics.md) first (setup + the core concepts). Keep [`docs/git-cheatsheet.md`](docs/git-cheatsheet.md) open while working, nobody remembers all the commands.

---

## tl;dr

1. **Never** commit directly to `main` or `dev`.
2. Every task gets its own branch, created from `dev`, named `feature/<what-you-do>`.
3. Run `git pull` before you start working.
4. Commit small and often, push your branch to GitHub.
5. Open a **Pull Request into `dev`** (not into `main`!). Someone else looks at it, then it gets merged.
6. `dev` only goes into `main` when everything works and we all agree.
7. Never commit private keys, seed phrases or `.env` files. Not even for test wallets.

---

## The branches

| Branch | What it's for | Who pushes to it |
|---|---|---|
| `main` | The "official" version. Should always work. This is what we hand in. | Nobody directly, only via Pull Request from `dev` |
| `dev` | The working version. All finished features land here first. | Nobody directly, only via Pull Requests from feature branches |
| `feature/...` | One branch per task, e.g. `feature/lending-pool` or `feature/report-intro`. Deleted after merging. | You, as much as you want |

`feature/example-1` is just a demo so you can see what a feature branch looks like on GitHub. Don't build actual stuff on it.

Here's how it looks over time:

```
main     o--------------------------------------o        (hand-in ready)
          \                                    /
dev        o-----------o-----------------o----o
            \         /  \              /
feature/a    o---o---o    \            /
                           \          /
feature/b                   o---o----o
```

Each `o` is a commit (a saved snapshot). Feature branches split off `dev`, collect commits, and get merged back into `dev`. When `dev` is in a good state, it gets merged into `main`.

**Why so complicated?** Because this way nobody's half-finished work breaks the project for everyone else. You can mess around on your own branch as much as you want, `dev` and `main` stay clean.

---

## The cycle, step by step

### 0. One-time setup

Install git, connect it to GitHub, clone the repo. All explained in [`docs/git-basics.md`](docs/git-basics.md#setup-do-this-once).

### 1. Get the latest version of `dev`

```bash
git switch dev
git pull
```

Do this **every time** before you start something new. Otherwise you're building on an outdated version and will get conflicts later.

### 2. Create your feature branch

```bash
git switch -c feature/short-description
```

Naming rules: lowercase, words separated by `-`, describe the task. Examples:

- `feature/token-contract`
- `feature/uniswap-research`
- `feature/report-chapter-2`

Bad: `feature/stuff`, `feature/Max`, `test123`

### 3. Work and commit

Edit files like normal. Whenever you've finished a small, logical step, commit it:

```bash
git status                 # what did I change?
git add <file>             # or: git add .   (everything)
git commit -m "Add deposit function to LendingPool"
```

Commit often. A commit is basically a save point you can go back to. 5 small commits are better than 1 giant one at the end of the day.

### 4. Push your branch to GitHub

The first time:

```bash
git push -u origin feature/short-description
```

After that, just:

```bash
git push
```

Pushing doesn't affect anybody else. It just uploads your branch so it's backed up and visible to the others.

### 5. Open a Pull Request (PR)

When your task is done:

1. Go to the repo on GitHub. Usually there's a yellow banner "*feature/... had recent pushes*" with a **Compare & pull request** button. Otherwise: **Pull requests** tab → **New pull request**.
2. **Check the base branch!** It has to say `base: dev` ← `compare: feature/your-branch`. GitHub sometimes picks `main` as default, change it to `dev`.
3. Write a title and a short description: what did you do, is there anything the reviewer should look at.
4. On the right under **Reviewers**, add one of the others.
5. Click **Create pull request**.

### 6. Review

The reviewer opens the PR, clicks on **Files changed** and looks through it. Then either:

- **Approve** → good to merge
- **Request changes** / leave comments → the author fixes stuff

To fix things, just keep working on the same branch, commit and `git push`. The PR updates automatically, no need to open a new one.

### 7. Merge

Once approved: click **Merge pull request** → **Confirm merge** → **Delete branch**. Your work is now in `dev`.

### 8. Clean up locally

```bash
git switch dev
git pull
git branch -d feature/short-description
```

Then back to step 1 for the next task.

---

## `dev` → `main`

When `dev` is in a state we're happy with (milestone, before a deadline, before hand-in), someone opens a PR from `dev` into `main`. Everybody takes a look, then we merge. That's the only way anything gets into `main`.

---

## Your branch is outdated

If others merged stuff into `dev` while you were still working on your branch, pull those changes into your branch:

```bash
git switch feature/your-branch
git pull origin dev
```

This merges the latest `dev` into your branch. Do this before opening a PR, and also in between if your branch lives longer than a few days.

---

## Merge conflicts (don't panic)

A conflict happens when two people changed the **same lines** in the same file. Git can't decide which version is correct, so it asks you. The file will contain something like this:

```
<<<<<<< HEAD
uint256 public fee = 3;
=======
uint256 public fee = 5;
>>>>>>> dev
```

- Above `=======`: your version
- Below `=======`: the other version

To fix it:

1. Open the file, decide what the final version should be (one of them, or a mix).
2. Delete the markers `<<<<<<<`, `=======` and `>>>>>>>`.
3. Save, then:

```bash
git add <file>
git commit -m "Resolve merge conflict in LendingPool"
```

If you want to abort and go back to before the merge: `git merge --abort`.

VS Code shows conflicts with buttons like "Accept Current / Accept Incoming / Accept Both", which makes this way easier. If you're not sure which version is right, ask the person who wrote the other part.

---

## Commit messages

Short, in English, describe **what** the commit does. Write it like a command ("Add", "Fix", "Update", not "Added" or "Fixing").

| Good | Bad |
|---|---|
| `Add withdraw function to Vault` | `changes` |
| `Fix rounding error in interest calculation` | `asdf` |
| `Update README with setup instructions` | `final version` |
| `Add sources for chapter 2` | `fixed it (hopefully)` |

---
