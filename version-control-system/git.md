# 📝 Git & GitHub Notes

This file contains essential Git Commands and concepts for reference.  
Use it as a quick guide while learning or developing projects.


## 1. Git Configuration

| Command                                                   | Description                                           |
| --------------------------------------------------------- | ----------------------------------------------------- |
| `git config --global user.name "Your Name"`               | Sets your name for all commits (global)               |
| `git config --global user.email "your_email@example.com"` | Sets your email for all commits (global)              |
| `git config --global core.editor "code --wait"`           | Sets default editor (e.g., VS Code)                   |
| `git config --global init.defaultBranch main`             | Sets `main` as the default branch instead of `master` |
| `git config --global color.ui auto`                       | Enables colored Git output                            |
| `git config --list`                                       | Shows all Git configuration                           |
| `git config --global --edit`                              | Opens the global config file to edit                  |
| `git config --global fetch.prune true`                    | every git fetch will automatically remove stale branches |



## 2. Git Workflow

```
WORKING DIRECTORY ---> STAGING AREA ---> LOCAL REPO (Commits) ---> REMOTE REPO (GitHub)
```


## 3. Basic Git Commands

| Command                           | Description                                    |
| --------------------------------- | ---------------------------------------------- |
| `git init`                        | Initialize Git in current folder               |
| `git status`                      | Show repo status (staged, unstaged, untracked) |
| `git log`                         | Show commit history (detailed)                 |
| `git log --oneline`               | Show commit history (compact)                  |
| `git log --graph --oneline --all` | Visual commit tree (very useful!)              |


## 4. Working Directory → Staging area

| Command                         | Description                                                        |
| ------------------------------- | ------------------------------------------------------------------ |
| `git add file.txt`              | Stage changes of `file.txt`                                        |
| `git add .`                     | Stage all changes in current directory                             |
| `git rm --cached file.txt`      | Untrack file but keep it locally                                   |
| `git rm --cached .`             | Untrack all files but keep them locally                            |
| `git restore file.txt`          | Discard changes in working directory (revert to last commit/stage) |
| `git restore .`                 | Discard all changes in working directory                           |
| `git restore --staged file.txt` | Unstage file but keep changes in working directory                 |
| `git restore --staged .`        | Unstage all files                                                  |


## 5. Staging area → Local Repo

| Command                    | Description                                                    |
| -------------------------- | -------------------------------------------------------------- |
| `git commit -m "message"`  | Commit staged changes with message                             |
| `git commit -am "message"` | Stage modified files **and** commit (new files won’t be added) |
| `git reset --soft HEAD~n`  | Reset to `HEAD~n` → undo commits but keep changes **staged** |
| `git reset --mixed HEAD~n` | Reset to `HEAD~n` → undo commits but keep changes in **working directory** |
| `git reset --hard HEAD~n`  | Reset to `HEAD~n` → undo commits and delete changes permanently |
| `git revert <commit>`      | Create new commit that undoes changes of `<commit>`            |
| `git rebase -i HEAD~3`     | Interactive rebase (edit, squash, drop commits)                |
| `git rebase --abort`       | Cancel rebase                                                  |

> Commit history looks like: `... → HEAD~3 → HEAD~2 → HEAD~1 → HEAD`


## 6. Git Branches

| Command                      | Description                                        |
| ---------------------------- | -------------------------------------------------- |
| `git branch`                 | List local branches                                |
| `git branch new-branch`      | Create new branch                                  |
| `git branch -d branch-name`  | Delete branch (safe, only if merged)               |
| `git branch -D branch-name`  | Force delete branch                                |
| `git checkout branch-name`   | Switch branch                                      |
| `git checkout -b new-branch` | Create & switch to new branch                      |
| `git switch branch-name`   | Switch branch (same as checkout)  |
| `git switch -c new-branch` | Create & switch to new branch     |
| `git merge main`             | Merge `main` into current branch                   |
| `git merge --abort`          | Cancel merge (on conflict)                         |
| `git rebase main`            | Reapply commits on top of `main` (no merge commit) |
| `git rebase --abort`         | Cancel rebase                                      |

> - Common branch naming: `feature/`, `bugfix/`, `hotfix/`, `docs/`, `chore/`.
> - ⚠️ You cannot switch branches with uncommitted changes (unless stashed).


## 7. Git Stash (store staging area and working directory)

| Command                     | Description                             |
| --------------------------- | --------------------------------------- |
| `git stash`                 | Save uncommitted changes                |
| `git stash list`            | Show stashes                            |
| `git stash pop`             | Apply latest stash **and remove it from stash list** |
| `git stash apply stash@{0}` | Apply stash but keep it in stash list                 |
| `git stash drop stash@{0}`  | Delete specific stash                   |
| `git stash clear`           | Delete all stashes                      |


## 8. Git Comparison

| Command                              | Description                              |
| ------------------------------------ | ---------------------------------------- |
| `git diff`                           | Compare working directory vs last commit |
| `git diff --staged`                  | Compare staged changes vs last commit    |
| `git diff <commit1> <commit2>`       | Compare two commits                      |
| `git diff branch1..branch2`          | Compare two branches                     |
| `git log branch1..branch2`           | Show commits in branch2 not in branch1   |
| `git log --oneline branch1..branch2` | Same but compact view                    |


## 9. GitHub SSH Setup

1. **Generate SSH key**

   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
2. **Start agent & add key**

   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```
3. **Copy public key**

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
4. **Add key to GitHub**
   Go to **Settings → SSH and GPG keys → New SSH key**, paste, and save.

> 🔑 SSH setup may change — always check GitHub Docs for latest steps.


## 10. Local Repo → Remote Repo

| Command | Description                                          |
| ----------------------------------- | ---------------------------------------------------- |
| `git clone <url>`                   | Download repo                                        |
| `git remote add origin <url>`       | Add remote `origin`                                  |
| `git remote rename origin new-name` | Rename remote                                        |
| `git remote remove origin`          | Remove remote                                        |
| `git remote -v`                     | Show remotes                                         |
| `git branch -r`                     | List remote branches                                 |
| `git branch -a`                     | List all branches (local + remote)                   |
| `git push origin main`              | Push `main` to remote                                |
| `git push origin main:dev`          | Push local `main` → remote `dev`                     |
| `git push -u origin main`           | Push & set upstream (next time `git push` is enough) |
| `git push --delete origin branch`   | Delete remote branch                                 |
| `git fetch origin`                  | Fetch updates (does not merge)                       |
| `git fetch --prune origin`          | Removes any remote-tracking branches locally that no longer exist on the remote |
| `git pull origin main`              | Fetch + merge `main` into current branch             |


## 11. How to Contribute to a Project

1. **Fork** the original repo → your GitHub.
2. **Clone** your fork locally.
3. Add remotes:

   * `origin` → your fork
   * `upstream` → original repo
4. Keep `main` updated:

   ```bash
   git fetch upstream
   git rebase upstream/main
   ```
5. Create a new branch (`feature/...`) and work on it.
6. Before pushing:

   ```bash
   git fetch upstream
   git rebase upstream/main   # update your main
   git checkout feature/xyz
   git rebase main            # update feature branch on top of main

   ```
7. Push your branch to `origin`.
8. Open a **Pull Request** (PR) from your fork → upstream repo.
9. Once merged, delete your feature branch locally & remotely.
