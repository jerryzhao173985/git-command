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

