# Index — commands & topics

1. `git config --global user.name` / `user.email`
2. `ssh-keygen -t rsa -b 4096 -C "..." `
3. `ls ~/.ssh` / `cat ~/.ssh/id_rsa.pub`
4. `git init`
5. `git status`
6. `git add <file>`
7. `git commit -m "..."`
8. `git log` / `git log --oneline`
9. `git remote add origin <URL>`
10. `git branch -M master` / `git branch -M main`
11. `git push origin master` / `git push -u origin main`
12. `git pull origin master`
13. `git clone <URL>`
14. `git mv` / `git rm` / `git rm --cached`
15. `git restore --staged` / `git restore`
16. `.gitignore` (create + commit)
17. `git diff`
18. `git status` (again — the daily habit)
19. `git commit -a` and `git commit --amend` (concept mention)
20. `git checkout` / `git switch` (concept mention)

---

## What Git is (one-liner)

Git is a distributed version control system that tracks snapshots of your project (code, scripts, small data files) so you can reproduce, branch, and collaborate.
How files move through Git (step-by-step mental model)
~~~

1. Working Directory
This is your real folder.
Files here are edited using:
```
nano

echo >>
```
VS Code / editor

Example:
create.txt   (you just edited this)

At this point:
Git knows the file changed
But it is NOT saved in history

2. Staging Area (Index)

Temporary holding area.
You are telling Git:
“This exact version is what I want to commit.”

Command:
```
git add create.txt
```

Now:
File is staged
Git will include this version in the next commit

3. Local Repository (.git folder)
Permanent snapshots (commits).
This is your time machine.

Command:
```
git commit -m "added first version of create.txt"
```

Now:
File is safely stored in history
You can go back to it anytime

4. Remote Repository (GitHub)

Shared copy of your commits.
Used for:
backup
collaboration
resume / portfolio
```
Command:

git push origin master
```

Now:

Your local commits are visible on GitHub

---

# SECTION 1: Setup & identity

## `git config --global user.name "yourname"`

Set your name for commits (global config).

~~~
git config --global user.name "anshikabisht45"
~~~

## `git config --global user.email "you@example.com"`

Set your email for commits (global config).

~~~
git config --global user.email "anshikabisht45@gmail.com"
~~~

## `ssh-keygen -t rsa -b 4096 -C "email"`

Create an SSH keypair to authenticate with GitHub without typing a password.

* `-t rsa`: algorithm  
* `-b 4096`: key length (strong)  
* `-C`: comment (usually email)

~~~
ssh-keygen -t rsa -b 4096 -C "anshikabisht45@gmail.com"
~~~

## `ls ~/.ssh`

Check whether keys were created.

~~~
ls ~/.ssh
~~~

## `cat ~/.ssh/id_rsa.pub`

Show the public key to copy into GitHub SSH keys.

~~~
cat ~/.ssh/id_rsa.pub
~~~

---

# SECTION 2: Basic local repo workflow

## `git init`

Initialize an empty git repository.

~~~
git init
~~~

## `touch create.txt random.txt`

Create files for testing.

~~~
touch create.txt random.txt
~~~

## `git status`

Show current branch, staged, unstaged, and untracked files.

~~~
git status
~~~

## `git add <file>`

Stage files into the index.

~~~
git add create.txt
~~~

## `git add .gitignore`

Add ignore rules.

~~~
git add .gitignore
~~~

## `git commit -m "message"`

Create a commit snapshot.

~~~
git commit -m "this is my 1st commit"
~~~

## `git log`

View commit history.

~~~
git log
~~~

## `git log --oneline`

Compact commit history.

~~~
git log --oneline
~~~

---

# SECTION 3: Remote and push

## `git remote add origin <URL>`

Link local repo to GitHub.

~~~
git remote add origin https://github.com/anshikabisht45/git-test.git
~~~

## `git branch -M master`

Rename branch.

~~~
git branch -M master
~~~

## `git push origin master`

Push commits to GitHub.

~~~
git push origin master
~~~

## `git pull origin master`

Pull updates from remote.

~~~
git pull origin master
~~~

## `git clone <URL>`

Clone a remote repository.

~~~
git clone https://github.com/anshikabisht45/git-test.git
~~~

---

# SECTION 4: Working tree vs staging vs repo

* **Working directory** — files you edit  
* **Staging area (index)** — snapshot being prepared  
* **Local repository** — committed history  
* **Remote** — GitHub  

Flow: **work → stage → commit → push**

---

# SECTION 5: Fixing staged / tracked files

## `git restore --staged <file>`

Unstage a file.

~~~
git restore --staged create.txt
~~~

## `git restore <file>`

Discard local changes.

~~~
git restore create.txt
~~~

## `git rm <file>`

Remove file and stage deletion.

~~~
git rm random.txt
~~~

## `git rm --cached <file>`

Stop tracking file but keep locally.

~~~
git rm --cached sample1.fastq
~~~

## `git mv oldname newname`

Rename file and stage change.

~~~
git mv create.txt create.py
~~~

## Error: file has changes staged in the index

Fix options:

~~~
git rm --cached random.txt
# or
git rm -f random.txt
~~~

---

# SECTION 6: .gitignore

Create rules for files you never want to track.

~~~
echo "sample1.fastq" >> .gitignore
git add .gitignore
git commit -m "updated a gitignore file"
~~~

---

# SECTION 7: Inspecting changes

## `git diff`

Show unstaged changes.

~~~
git diff
~~~

## `git status`

Your daily GPS.

~~~
git status
~~~

---

# SECTION 8: Workflow I used

* create → `touch create.txt random.txt`
* init → `git init`
* add → `git add create.txt`
* commit → `git commit -m "this is my 1st commit"`
* ignore → `.gitignore`
* push → `git push origin master`

---

# SECTION 9: Where this appears in real pipelines

* Pipeline versioning (Nextflow / Snakemake)
* Reproducibility & audits
* CI/CD & automation
* Team collaboration (Elucidata-style workflows)
~~~
---

# Interview Questions — Lecture 4

# 1. What problem does Git actually solve?

Answer (interview-style):
Git solves the problem of tracking, reproducing, and collaborating on changes over time. Instead of manually managing file versions, Git creates a history of snapshots that lets you:

revert mistakes,

compare changes,

collaborate without overwriting others’ work.

In data science and bioinformatics, this is critical because analyses evolve iteratively and must remain reproducible.

# 2. Explain the difference between Git and GitHub.

Answer:
Git is a local version control system that tracks changes on my machine.
GitHub is a remote hosting platform that stores Git repositories and enables collaboration, backup, and review.

Git works without GitHub. GitHub does not work without Git.

# 3. What are the four states of a file in Git?

Answer:
A file can exist in:

Working Directory – edited but not tracked

Staging Area (Index) – selected for the next commit

Local Repository – committed and saved in history

Remote Repository – pushed and shared on GitHub

Understanding these states prevents accidental data loss and broken commits.

# 4. Why does Git have a staging area at all?

Answer:
The staging area allows intentional commits.
I can stage only the changes that logically belong together, even if I modified many files.

This is essential in pipelines where you may experiment freely but only want to commit stable steps.

# 5. What does git status actually tell you?

Answer:
git status is a diagnostic command. It tells me:

which branch I’m on,

which files are staged,

which files are modified but not staged,

which files are untracked.

It’s the first command I run when something feels “off.”

# 6. Why did Git refuse git rm random.txt in your session?

Answer:
Because the file had changes staged in the index.
Git blocks destructive actions until you explicitly decide whether to:

keep the file staged (--cached)

or force removal (-f)

This safeguard prevents accidental loss of tracked data.

# 7. Difference between git restore and git restore --staged?

Answer:

git restore file → resets the working directory copy

git restore --staged file → removes the file from the staging area only

This distinction is critical when you realize you staged the wrong version of a file.

# 8. Why did Git show “Changes to be committed” and “Changes not staged” for the same file?

Answer:
Because I staged one version of the file, then edited it again afterward.

Git is showing two realities at once:

the staged snapshot

the newer working copy

This is expected behavior, not an error.

# 9. What does git diff show?

Answer:
git diff shows the difference between:

working directory ↔ staging area (by default)

It answers the question:

“What exactly have I changed?”

This is invaluable before committing experimental edits.

# 10. Why is committing frequently considered good practice?

Answer:
Frequent commits:

reduce risk,

improve traceability,

make debugging easier,

allow selective rollback.

In bioinformatics, this helps isolate which change introduced an error in a pipeline or analysis.

# 11. What is the purpose of .gitignore?

Answer:
.gitignore tells Git which files should never be tracked, such as:

raw FASTQ files,

large intermediate outputs,

temporary logs.

This keeps repositories lightweight, clean, and reviewable.

# 12. Why should raw biological data usually not be pushed to GitHub?

Answer:
Because:

files are large,

they change rarely,

Git is inefficient for binary data.

Instead, Git should track code, metadata, and configuration, while data lives in storage systems or is referenced via paths.

# 13. What happens when you rename a file using git mv?

Answer:
Git records it as a rename operation, preserving file history.

This is important because history continuity matters when tracking the evolution of scripts or notebooks.

# 14. Why didn’t your second commit appear in git log earlier?

Answer:
Because no commit had actually succeeded yet.
Git only updates history after a successful git commit.

Staging files alone does not create history.

# 15. What does git branch -M master do?

Answer:
It renames the current branch forcefully to master.
This ensures consistency between local and remote branch names.

# 16. What does git push origin master do?

Answer:
It sends local commits from the master branch to the remote repository named origin.

Nothing is shared until this step happens.

# 17. What does git pull do internally?

Answer:
git pull is actually two commands:

git fetch

git merge

It updates the local branch with changes from the remote.

# 18. Why is Git essential for reproducible science?

Answer:
Because it:

preserves exact code states,

records when and why changes occurred,

allows others to reproduce analyses step-by-step.

Reproducibility is impossible without version control.

# 19. How does Git help in team-based data science?

Answer:
It enables:

parallel work without overwriting,

code review via pull requests,

clear ownership of changes.

This is foundational in production-grade analytics teams.

# 20. One-line answer if an interviewer asks: “How comfortable are you with Git?”

Answer:
“I understand Git’s internal states — working directory, staging, commits, and remote — and can confidently debug versioning issues during active development.”

That line alone = strong signal 🔥

---

# Quick cheatsheet

~~~
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR_EMAIL"
ssh-keygen -t rsa -b 4096 -C "youremail@example.com"

git init
git status
git add file.txt
git commit -m "message"

git remote add origin https://github.com/USER/REPO.git
git branch -M master
git push origin master

git restore --staged file.txt
git restore file.txt
git rm --cached bigfile.fastq
echo "bigfile.fastq" >> .gitignore
git add .gitignore
~~~
git commit -m "ignore bigfile"
~~~
