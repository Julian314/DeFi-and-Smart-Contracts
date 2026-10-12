# Git basics

This file explains what git is, how it works and how you set it up. Once you're done here, read [`../CONTRIBUTING.md`](../CONTRIBUTING.md) for how we use it in this project.

---

## What is git?

You probably know this:

```
Seminararbeit.docx
Seminararbeit_v2.docx
Seminararbeit_v2_final.docx
Seminararbeit_v2_final_REALLY_FINAL_corrections.docx
```

Git solves exactly that problem, just properly. It's a **version control system**: it keeps track of every change to every file in a project, who made it, when and why. You can always go back to an older version, compare versions, and several people can work on the same files at the same time without overwriting each other.

Some key points:

- Git saves **snapshots** of the whole project, called **commits**. Every commit has a message describing what changed.
- Git works **locally**. Your laptop has the full project with its full history. You don't need internet for most things.
- It works with any files, but it's best with text files (code, Markdown, CSV). For Word or Excel files it can only say "something changed", not *what* changed.

## Git vs. GitHub

These are two different things:

- **Git** is the program on your computer that does the version control.
- **GitHub** is a website that hosts git repositories online. It's the central place where we share our work. It also adds features like Pull Requests, issues and a nice web interface.

Think of git like the email program, and GitHub like the mail server.

---

## Core concepts

### Repository (repo)

A project folder tracked by git. Contains your files plus a hidden `.git` folder where git stores the whole history. Don't touch `.git`.

### Commit

A saved snapshot of the project at one point in time. Each commit has:

- a unique ID (hash), e.g. `b1ae588`
- an author and a timestamp
- a message, e.g. `Add deposit function`
- a pointer to the previous commit

All commits together form the history of the project.

### The three areas

This is the part that confuses everyone at first. Your files go through three stages:

```
  Working directory        Staging area            Repository
  (your files)             ("ready to commit")     (history)

   edit files   --git add-->   staged    --git commit-->   committed
```

1. **Working directory**: the files as you see them in your folder. You edit them normally.
2. **Staging area**: a list of changes you've marked for the next commit (`git add`). This lets you choose which changes go into a commit, instead of always committing everything.
3. **Repository**: once you commit, the staged changes are saved permanently in the history (`git commit`).

### Branch

A parallel version of the project. You create a branch, make changes there, and the other branches don't notice anything. Later you merge your branch back. That's how several people can work at the same time. Our branch setup is explained in [`CONTRIBUTING.md`](../CONTRIBUTING.md#the-branches).

### Remote / origin

The copy of the repo on GitHub. `origin` is just the default name git uses for it.

### Clone, pull, push

```
           GitHub (origin)
            ^          |
   git push |          | git pull
            |          v
           your laptop (local)
```

- `git clone`: download the repo for the first time
- `git pull`: get the latest changes from GitHub
- `git push`: upload your commits to GitHub

Important: committing only saves **locally**. Nobody sees your commits until you push.

### Merge

Combining the changes from one branch into another. Usually git does this automatically. If two people changed the same lines, you get a **merge conflict** and have to decide manually (see [`CONTRIBUTING.md`](../CONTRIBUTING.md#merge-conflicts-dont-panic)).

### Pull Request (PR)

A GitHub feature, not a git one. A request saying "please merge my branch into `dev`". Others can look at the changes, comment and approve before it gets merged. Basically code review.

---

## Setup (do this once)

### 1. GitHub account + invite

- Create an account on [github.com](https://github.com) if you don't have one.
- Accept the invite to the repo. You'll get it by email, or go to [github.com/Julian314/DeFi-and-Smart-Contracts/invitations](https://github.com/Julian314/DeFi-and-Smart-Contracts/invitations). The repo is private, so you can't see it without accepting.

### 2. Install git

- **Windows**: download from [git-scm.com](https://git-scm.com/download/win) and install. The default options are fine. This also installs **Git Bash**, a terminal where all commands from our docs work, and the Git Credential Manager (needed for step 4).
- **macOS**: open the Terminal app and run `git --version`. If git isn't installed, macOS asks you to install the developer tools, click Install. Alternatively, with [Homebrew](https://brew.sh): `brew install git`.

Check that it works:

```bash
git --version
```

### 3. Tell git who you are

This name and email will appear on your commits. Use the email from your GitHub account.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

The last line just sets how `git pull` behaves, so git doesn't throw a warning at you later.

### 4. Log in to GitHub

GitHub doesn't accept your normal password for git. You need to log in once through the browser:

- **Windows**: nothing to do now. The first time you `git clone` or `git push`, a window pops up and you log in to GitHub in the browser.
- **macOS**: install the GitHub CLI (`brew install gh`, or from [cli.github.com](https://cli.github.com)), then run:

  ```bash
  gh auth login
  ```

  Choose: GitHub.com → HTTPS → Yes (authenticate git) → Login with a web browser.

### 5. Clone the repo

Go to the folder where you want the project (e.g. your uni folder), then clone:

```bash
cd ~/Documents/Uni
git clone https://github.com/Julian314/DeFi-and-Smart-Contracts.git
cd DeFi-and-Smart-Contracts
git switch dev
```

Done. Now follow the cycle in [`CONTRIBUTING.md`](../CONTRIBUTING.md#the-cycle-step-by-step).

---

## Terminal survival kit

All git commands are typed in a terminal (Git Bash on Windows, Terminal on macOS). You only need a few commands to get around:

| Command | Does |
|---|---|
| `pwd` | Shows which folder you're currently in |
| `ls` | Lists the files in the current folder |
| `cd foldername` | Go into a folder |
| `cd ..` | Go one folder up |
| `cd ~` | Go to your home folder |
| `clear` | Clears the screen |
| Arrow up | Repeat the previous command |
| `Tab` | Auto-completes file and folder names |
| `Ctrl + C` | Cancel the running command |

Git commands only work if you're **inside** the repo folder. If git says `fatal: not a git repository`, you're in the wrong folder. `cd` into the project.

If git opens a weird text editor (usually Vim, happens with `git pull` sometimes): type `:wq` and press Enter to save and exit.

## Prefer clicking?

Totally fine. These tools do the same thing with buttons:

- [GitHub Desktop](https://desktop.github.com): simplest option, made for beginners
- [VS Code](https://code.visualstudio.com): the "Source Control" tab in the sidebar does commit/push/pull/branches. Also a good editor for Solidity and Markdown.

The concepts are exactly the same, only the buttons differ. Our docs use the terminal commands because they're the same for everyone and easier to google.

---

## Sources and further reading

Start with the first two, the rest is for when you want to dig deeper.

- **Learn Git Branching**: interactive game in the browser that visualizes branches. Best way to actually understand them. [learngitbranching.js.org](https://learngitbranching.js.org)
- **GitHub Docs, Getting started**: official beginner guides incl. Pull Requests. [docs.github.com/en/get-started](https://docs.github.com/en/get-started)
- **Pro Git** (Chacon & Straub): the official git book, free. Chapters 1-3 cover everything we need. [git-scm.com/book](https://git-scm.com/book/en/v2)
- **Atlassian: Feature Branch Workflow**: explains the workflow we use. [atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- **A successful Git branching model** (Vincent Driessen): the original article behind the main/dev/feature setup ("Git Flow"). [nvie.com/posts/a-successful-git-branching-model](https://nvie.com/posts/a-successful-git-branching-model/)
- **MIT Missing Semester, Version Control**: lecture explaining how git works internally. [missing.csail.mit.edu/2020/version-control](https://missing.csail.mit.edu/2020/version-control/)
- **GitHub Git Cheat Sheet** (PDF). [education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)
- **Dangit, Git!?!**: how to fix the most common git mistakes. [dangitgit.com](https://dangitgit.com)
