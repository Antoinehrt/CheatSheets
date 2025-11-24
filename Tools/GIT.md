# Git - Version Control System

> **Description**: Git is a free and open-source distributed version control system designed to handle projects of all sizes efficiently. It allows tracking source code changes, collaborating with other developers, and maintaining a complete version history.

#Git #VersionControl #GitHub #Collaboration #Repository

---

## 📋 **Table of Contents**

- [SETUP](#setup) - User configuration
- [SETUP & INIT](#setup--init) - Repository initialization  
- [STAGE & SNAPSHOT](#stage--snapshot) - Working with changes
- [REMOTE SETUP](#remote-setup) - Connecting to remotes
- [BRANCH & MERGE](#branch--merge) - Branch workflows
- [INSPECT & COMPARE](#inspect--compare) - History and diffs
- [TRACKING PATH CHANGES](#tracking-path-changes) - File operations
- [.gitignore](#gitignore) - Ignoring files
- [SHARE & UPDATE](#share--update) - Remote operations
- [UNDO CHANGES](#undo-changes) - Fixing mistakes
- [REWRITE HISTORY](#rewrite-history) - Advanced operations
- [TEMPORARY COMMITS](#temporary-commits) - Stashing

---

## Main git commands

## SETUP

### Configuring user information used across all local repositories

**1.** Set a name that is identifiable for credit when review version history

```bash
git config --global user.name "<yourName>"
```

**2.** Set an email address that will be associated with each history marker

```bash
git config --global user.email "<yourEmail>"
```

**3.** Set automatic command line colouring for Git for easy reviewing

```bash
git config --global color.ui auto
```

## SETUP & INIT

### Configuring user information, initializing and cloning repositories

**1.** Initialize an existing directory as a Git repository

```bash
git init
```

**2.** Retrieve an entire repository from a hosted location via URL

```bash
git clone <url>
```

## STAGE & SNAPSHOT

### Working with snapshots and the Git staging area

**1.** Show modified files in working directory, staged for your next commit

```bash
git status
```

**2.** Add a file as it looks now to your next commit (stage)

```bash
git add <filename>
```

or to add every files

```bash
git add .
```

**3.** Unstage a file while retaining the changes in working directory

```bash
git reset <filename>
```

**3b.** Unstage a file while retaining the changes (modern approach)

```bash
git restore --staged <filename>
```

**4.** Diff of what is changed but not staged

```bash
git diff
```

**5.** Diff of what is staged but not yet committed

```bash
git diff --staged
```

**6.** Commit your staged content as a new commit snapshot

```bash
git commit -m "<yourMessage>"
```

## REMOTE SETUP

### From a new repository

```bash
git branch -M main
git remote add origin <linkOfTheRemoteRepo>
git push -u origin main
```

### Or push an existing repository

```bash
git remote add <origin> <linkOfTheRemoteRepo>
git branch -M main
git push -u origin main
```

## BRANCH & MERGE

### Isolating work in branches, changing context, and integrating changes

**1.** List your branches. a * will appear next to the currently active branch

```bash
git branch
```

**2.** Create a new branch at the current commit

```bash
git branch <branchName>
```

**3.** Switch to another branch and check it out into your working directory

```bash
git checkout
```

**4.** Merge the specified branch’s history into the current one

```bash
git merge <branchName>
```

**5.** Show all commits in the current branch’s history

```bash
git log
```

## INSPECT & COMPARE

### Examining logs, diffs and object information

**1.** Show the commit history for the currently active branch

```bash
git log
```

**2.** Show the commits on branchA that are not on branchB

```bash
git log branchB..branchA
```

**3.** Show the commits that changed file, even across renames

```bash
git log --follow <filename>
```

**4.** Show the diff of what is in branchA that is not in branchB

```bash
git diff branchB...branchA
```

**5.** Show any object in Git in human-readable format

```bash
git show <SHA>
```

### git adog

`git adog` is a useful custom alias for visualizing your repository's commit history in a clear and concise way. It combines several Git log options to provide a compact, one-line-per-
commit view with branch decorations and a graphical representation of the commit tree.

**1.** What does `git adog` do?

When you run `git adog`, it executes the following command:

```bash
git log --all --decorate --oneline --graph
```

This command includes:

- `--all`: Displays commits from all branches.
- `--decorate`: Shows references (e.g., branches, tags) associated with each commit.
- `--oneline`: Reduces each commit to a single line for a more compact view.
- `--graph`: Adds a graphical representation of the commit tree structure.

This makes it easier to track branching, merging, and the overall repository history at a glance.

**2.** How to create the alias for `git adog`?

To set up the `git adog` alias, use the following command:

```bash
git config --global alias.adog 'log --all --decorate --oneline --graph'
```

Once this alias is set, you can simply type `git adog` to visualize your repository's history.

## TRACKING PATH CHANGES

### Versioning file removes and path changes

**1.** Delete the file from project and stage the removal for commit

```bash
git rm <filename>
```

**2.** Change an existing file path and stage the move

```bash
git mv <existingPath> <newPath>
```

**3.** Show all commit logs with indication of any paths that moved

```bash
git log --stat -M
```

## .gitignore

### Preventing unintentional staging or committing of files

**1.** Save a file with desired patterns as .gitignore with either direct string matches or wildcard globs.

```bash
logs/
*.notes
pattern*/
```

**2.** System wide ignore pattern for all local repositories

```bash
git config --global core.excludesfile <filename>
```

### Generate .gitignore files online

For project-specific .gitignore files, visit [gitignore.io](https://www.toptal.com/developers/gitignore) to generate comprehensive .gitignore templates for your specific technology stack (Node.js, Python, Java, IDE, OS, etc.). 

## SHARE & UPDATE

### Retrieving updates from another repository and updating local repos

**1.** Fetch down all the branches from that Git remote

```bash
git fetch <remote>
```

**2.** Merge a remote branch into your current branch to bring it up to date

```bash
git merge <remote>/<branch>
```

**3.** Transmit local branch commits to the remote repository branch

```bash
git push <remote> <branch>
```

**3b.** Force push (use with caution - overwrites remote history)

```bash
git push --force-with-lease <remote> <branch>
```

**4.** Fetch and merge any commits from the tracking remote branch

```bash
git pull
```

## UNDO CHANGES

### Safely undoing commits and changes

**1.** Undo the last commit (keep changes staged)

```bash
git reset --soft HEAD~1
```

**2.** Undo the last commit (keep changes unstaged)

```bash
git reset HEAD~1
```

**3.** Create a new commit that undoes changes from a specific commit

```bash
git revert <commit-hash>
```

**4.** Restore a file to its last committed state (modern)

```bash
git restore <filename>
```

**5.** Restore a file to its last committed state (legacy)

```bash
git checkout -- <filename>
```

## REWRITE HISTORY

### Rewriting branches, updating commits and clearing history

**1.** Apply any commits of current branch ahead of specified one

```bash
git rebase <branch>
```

**2.** Clear staging area, rewrite working tree from specified commit

```bash
git reset --hard <commit>
```

## TEMPORARY COMMITS

### Temporarily store modified, tracked files in order to change branches

**1.** Save modified and staged changes

```bash
git stash
```

**2.** List stack-order of stashed file changes

```bash
git stash list
```

**3.** Write working from top of stash stack

```bash
git stash pop
```

**4.** Discard the changes from top of stash stack

```bash
git stash drop
```
