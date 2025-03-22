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
