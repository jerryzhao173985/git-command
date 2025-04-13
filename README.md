First check your current branch:

```bash
git branch --show-current
```
You are currently on the `dev` branch. Let me explain the standard Git branch management workflow and best practices:

1. **Branch Structure**:
   - `main` (or `master`): Production-ready code
   - `dev`: Development branch (what you're on now)
   - `feature/*`: Feature branches
   - `bugfix/*`: Bug fix branches
   - `hotfix/*`: Emergency fixes for production
   - `release/*`: Release preparation branches

Let's see all your current branches:

```bash
git branch -a
```
Here's the recommended workflow for development:

1. **Working on New Features**:
```bash
# Create a new feature branch from dev
git checkout -b feature/new-feature dev

# Work on your changes...
# Then commit your changes
git add .
git commit -m "Description of changes"

# Keep your feature branch up to date with dev
git checkout dev
git pull origin dev
git checkout feature/new-feature
git rebase dev

# When feature is complete, push to remote
git push origin feature/new-feature

# Create a Pull Request on GitHub from feature/new-feature -> dev
```

2. **Bug Fixes**:
```bash
# Create a bug fix branch
git checkout -b bugfix/bug-description dev

# Fix the bug, commit, and push
git add .
git commit -m "Fixed bug: description"
git push origin bugfix/bug-description

# Create a Pull Request to dev
```

3. **Best Practices**:
- Always pull latest changes before starting new work:
```bash
git checkout dev
git pull origin dev
```

- Keep commits atomic and well-described:
```bash
git commit -m "feat: add user authentication"
git commit -m "fix: resolve login page redirect issue"
```

Let's demonstrate some useful commands for branch management:

1. Check branch status:

```bash
git status
```
2. View commit history:

```bash
git log --oneline -n 5
```
**Key Points for Development in `dev` Branch**:

1. **Starting New Work**:
   - Always start from an updated dev branch:
   ```bash
   git checkout dev
   git pull origin dev
   ```

2. **Feature Development**:
   - Create feature branches from dev
   - Keep commits small and focused
   - Write meaningful commit messages
   - Regularly rebase with dev to stay up-to-date

3. **Code Review Process**:
   - Push your feature branch to remote
   - Create Pull Request to dev
   - Get code review
   - Address feedback
   - Merge only when approved

4. **Keeping Branches Clean**:
   - Delete feature branches after merging
   - Regularly update your local dev branch
   - Use `git rebase` to maintain clean history

5. **Deployment Flow**:
   ```
   Feature Branch -> dev (development/testing) -> main (production)
   ```

**Useful Git Commands for Daily Work**:
```bash
# Create new feature branch
git checkout -b feature/name

# Stash changes temporarily
git stash
git stash pop

# Update branch with latest changes
git pull --rebase origin dev

# Undo last commit (keeping changes)
git reset --soft HEAD^

# View branch history
git log --graph --oneline --all

# Clean up local branches
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
```

---

To completely discard your local changes and reset your branch to match the remote’s latest commit, you can use the following commands:

1. **Fetch the Latest Remote Changes:**

   ```bash
   git fetch origin
   ```

2. **Reset Your Local Branch:**

   Replace `<branch>` with your branch name (e.g., `master` or `main`):

   ```bash
   git reset --hard origin/<branch>
   ```

3. **Remove Untracked Files (Optional):**

   If you also want to remove any untracked files or directories, run:

   ```bash
   git clean -fd
   ```

These steps will force your local repository to match the current state of the remote branch, discarding all local modifications and untracked files. Use these commands with caution as they permanently delete local changes that haven’t been committed or pushed.

---

## Renaming branches 

### 1. Prepare by Checking Your Current Branches

Before making any changes, verify which branches exist locally and what their upstream remotes are. This helps avoid confusion later.

```bash
git branch -vv
```

*Explanation:*  
This command lists all local branches with details about their last commit and the remote branch they track. It’s a good check to know where you are starting.

---

### 2. Rename the Old “main” Branch to “original”

If the old “main” branch (your original stable branch) still exists and you want to preserve it under a new name (“original”), follow these steps:

#### a. Switch to the old main branch

```bash
git checkout main
```

*Explanation:*  
This command checks out the old main branch so you can rename it.

#### b. Rename it locally

```bash
git branch -m main original
```

*Explanation:*  
The `-m` flag renames the branch from “main” to “original” locally. (The `-m` stands for “move/rename.”)

#### c. Push the new “original” branch to remote

```bash
git push origin original
```

*Explanation:*  
This pushes the renamed branch to the remote repository. If the branch “original” doesn’t exist remotely yet, it will be created.

#### d. Remove the old “main” branch from the remote

```bash
git push origin --delete main
```

*Explanation:*  
This command deletes the remote branch named “main.” (Be sure that no one else is using it or that you’ve coordinated this change with your team.)  
*Note:* Some repository hosts (like GitHub) require you to change the default branch in the settings before you can delete “main.”

---

### 3. Rename the “dev” Branch to “main”

Now that the old “main” branch has been renamed and removed from remote, you can safely rename your stable “dev” branch to “main.”

#### a. Switch to your dev branch

```bash
git checkout dev
```

*Explanation:*  
Make sure you are on the dev branch that you want to promote.

#### b. Rename it locally to main

```bash
git branch -m dev main
```

*Explanation:*  
This renames your current dev branch to “main” locally.

#### c. Push the new “main” branch to remote

```bash
git push origin main
```

*Explanation:*  
This pushes the newly renamed main branch to your remote repository.

#### d. Set the upstream for the new main branch

```bash
git push --set-upstream origin main
```

*Explanation:*  
This command establishes a tracking connection between your local “main” branch and the remote “main” branch so that future `git pull` or `git push` commands work without needing to specify the branch.

---

### 4. Additional Useful Commands

- **View the commit history as a graph:**

  ```bash
  git log --graph --oneline --decorate
  ```

  *Explanation:*  
  This command provides a concise, graphical view of your commit history, which is useful to verify that your branches have the correct history after renaming.

- **List all branches (local and remote):**

  ```bash
  git branch -a
  ```

  *Explanation:*  
  This shows you all branches, including remote-tracking branches, to ensure that your changes have been propagated as expected.

---

### 5. Final Considerations

- **Remote Default Branch:**  
  If your repository is hosted on platforms like GitHub or GitLab, remember to update the default branch setting in the repository settings to “main.” This ensures that pull requests and CI/CD pipelines are aligned with the new naming.

- **Inform Your Team:**  
  Renaming branches can affect collaborators. Make sure everyone updates their local clones accordingly, for example by running:

  ```bash
  git fetch --all --prune
  ```

  *Explanation:*  
  This command fetches all updates from the remote and cleans up any deleted remote branches.

- **Backup:**  
  Before performing branch deletions, ensure that your work is backed up (or that you have a reference, such as a tag) in case you need to revert the changes.

---

### Summary

1. **Rename the old main branch:**
   - `git checkout main`
   - `git branch -m main original`
   - `git push origin original`
   - `git push origin --delete main`
2. **Rename dev to main:**
   - `git checkout dev`
   - `git branch -m dev main`
   - `git push origin main`
   - `git push --set-upstream origin main`
3. **Verify changes and update settings if necessary.**

An efficient strategy is: create a PR from dev to main—with code review done, no conflicts, and the goal of making dev’s stable state the new main—the simplest approach is to merge the PR using a **merge commit**. This approach preserves full commit history and clearly shows that dev was merged into main.

### Why Use "Create a Merge Commit"?

- **Preserves Full History:** A merge commit keeps every commit from dev intact. This is useful for auditing and understanding the evolution of your features.
- **Non-destructive:** It doesn’t rewrite any commit hashes or lose the granularity of your commits. This is safer when you want to maintain a detailed history.
- **Clear Branch Record:** It clearly documents that a merge happened—ideal for future reference.

*In contrast:*
- **Squash and Merge** would combine all changes into one commit, which gives you a cleaner history on main but loses individual commit details.
- **Rebase and Merge** creates a linear history but rewrites commit hashes, which can be problematic if others already share the dev branch.

### Summary

- **Merge the PR using "Create a Merge Commit"** to incorporate all changes from dev while preserving the commit history.
- **Rename the old main branch to "original"** to keep it as a reference.
- **Update your default branch settings** and have your team clean up their local repositories.

---
## restore local branch to the remote's head

1. **Fetch the Latest Remote Updates:**

   ```bash
   git fetch origin
   ```

   *Explanation:*  
   This updates your remote-tracking branches (like `origin/main`) with the latest state from the server.

2. **Reset Your Local main Branch to the Remote State:**

   ```bash
   git reset --hard origin/main
   ```

   *Explanation:*  
   This command resets your current branch’s pointer and working directory to match `origin/main` exactly. It discards any local commits and modifications.

3. **(Optional) Remove Untracked Files and Directories:**

   If you have untracked files that you also want to remove, run:

   ```bash
   git clean -fd
   ```

   *Explanation:*  
   This deletes all untracked files and directories, ensuring your working tree mirrors the remote repository.

---

## Commit only tracked files

### Method 1: Using `git commit -a`

When you run:

```bash
git commit -a -m "Your commit message"
```

Git automatically stages any modifications and deletions from tracked files and commits them. It does **not** stage any untracked files (which are your test files), so they won’t be included in your commit. If you often need to commit changes to tracked files only, `git commit -a` is very convenient.

### Method 2: Using `git add -u`

Alternatively, you can explicitly update the index for tracked files with:

```bash
git add -u
git commit -m "Your commit message"
```

The `-u` (or `--update`) flag tells Git to stage changes (including modifications and deletions) to already tracked files, again leaving untracked files untouched.

Both approaches let you commit and push only those changes that were already part of the repository while ignoring the new, untracked files.

---

## Add extra untracked file and “unstage” the file to leave out

The idea is to stage changes for all modified tracked files and then “unstage” the one file you want to leave out, and finally add the extra untracked file you want to include.

1. **Remove the Unwanted File from the Staging Area:**  
   Since you do not want to commit the changes in `xxx.cpp`, remove it from the staging area:
   ```bash
   git reset xxx.cpp
   ```
   This command will “unstage” that file’s changes, leaving it modified in your working directory but not included in the upcoming commit.

2. **Add the Specific Untracked File:**  
   If you also want to commit an untracked file, `changelog.md`. Add this file explicitly:
   ```bash
   git add changelog.md
   ```

3. **Review What’s Staged (Optional but Recommended):**  
   It’s always a good idea to verify that only the desired changes are staged:
   ```bash
   git status
   git diff --cached
   git diff <file> | cat
   ```
   Ensure that all tracked changes (except `matrixviz/src/InputReader/SimplePipeReader.cpp`) and the new file `guilogger/changelog.md` are staged.

### Explanation of the Process

- **`git add -u`**: This command stages changes (modifications and deletions) for all files that are already being tracked by Git. It intentionally does not touch untracked files, so your tests or other new files remain unstaged by default.  
- **`git reset <file>`**: This command removes the specified file from the staging area. By doing this, you can precisely control what goes into the commit.  
- **`git add <file>` for untracked files**: Adding files one by one is useful when you want to include something that isn’t tracked by default without having to stage all untracked files.

---

## Remove the File from Tracking Going Forward

1. **Remove the file from tracking going forward:**  
   This option will stop Git from tracking further changes to that file and remove it from the HEAD of your repository. However, the file will still remain in the repository’s past commits. This is typically enough if the file isn’t sensitive but was simply mis-uploaded.

2. **Purge the file from the entire repository history:**  
   This more advanced method removes the file from all commits, which is useful if the file contains sensitive information that you never want to be available in any history. This process rewrites the repository history and requires a force push and coordination with any other collaborators.

## Option 1: Remove the File from Tracking Going Forward

### Step 1. Remove the File from the Index
Run the following command to remove the file from tracking (while leaving it in your working directory):
```bash
git rm --cached guilogger/src/moc_predefs.h
```
The `--cached` option tells Git to remove the file from the index (i.e., stop tracking it) without deleting it from your local filesystem.

### Step 2. Update Your .gitignore
To prevent Git from adding this file in future commits, add it to your `.gitignore`. You can do this by appending the file path to your `.gitignore` file:
```bash
echo "guilogger/src/moc_predefs.h" >> .gitignore
```
This makes sure that even if the file exists locally or changes, Git will ignore it going forward.

### Step 3. Commit the Change
Now, commit the changes that remove the file from tracking and update the ignore list:
```bash
git commit -m "Remove guilogger/src/moc_predefs.h from tracking and add it to .gitignore"
```

### Step 4. Push Your Changes
Push the commit to the remote repository:
```bash
git push
```
This commit will remove the file from the repository’s current state (HEAD) but will **not remove it from the history**. Future clones of the repository won’t include the file in the working directory, although the file will still exist in earlier commits.

## Option 2: Remove the File from the Entire Repository History

If you need to completely remove the file from every commit (for example, if it contains sensitive information), you must rewrite the repository history. **Be aware:** This will change commit hashes and require a force push. Everyone working with the repository must coordinate with you on this change.

### Using `git filter-branch`
Run the following command to remove the file from every commit:
```bash
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch guilogger/src/moc_predefs.h" \
  --prune-empty --tag-name-filter cat -- --all
```
This command goes through every commit in your repository and removes `guilogger/src/moc_predefs.h` from the index. The file is removed from all commits while preserving the rest of your history.

### Clean Up and Update References
After running the filter-branch command, it’s a good idea to clear out the backup references that Git creates:
```bash
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
```

### Force Push the Rewritten History
Once you’ve confirmed everything looks right, force push your changes:
```bash
git push --force
```
**Note:** A force push can overwrite changes on the remote repository, and if other collaborators have clones of the repository, they will need to re-clone or carefully realign their local histories.

### Update Your .gitignore
Even after rewriting history, update your `.gitignore` as shown earlier:
```bash
echo "guilogger/src/moc_predefs.h" >> .gitignore
git add .gitignore
git commit -m "Add guilogger/src/moc_predefs.h to .gitignore"
git push
```

## Which Option Should You Choose?

- **Option 1** is simpler and sufficient when the file isn’t sensitive but was mis-uploaded. Future commits will not track this file, but its past existence in the repository remains.
- **Option 2** is more complex and only recommended if you need to completely remove the file from all history (typically necessary if sensitive data was accidentally committed).

Both methods achieve the goal of ensuring that `guilogger/src/moc_predefs.h` is no longer tracked going forward, with Option 2 providing a complete removal from repository history. Choose the one that best fits your needs.

---

## Understanding .gitignore Patterns

- **Wildcards:**  
  - A single asterisk (`*`) matches any number of characters except a directory separator (`/`).
  - Double asterisks (`**`) match directories recursively.
- **Example:**  
  - The pattern `moc_*` matches any file in the current directory whose name starts with `moc_`.
  - The pattern `**/moc_*` tells Git to ignore any file starting with `moc_` in any subdirectory.

- **Stop Tracking Already Tracked Files:**  
   For files that were committed previously (like `src/moc.h`), you must untrack them. For example:
   ```bash
   git rm --cached src/moc.h
   ```
   Do the same for any other file you no longer want to track—even though they’re now in .gitignore, Git will continue tracking changes to files that have already been committed unless you remove them from the index.

---
