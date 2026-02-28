# Setting Up Development Repository

## Step-by-Step Setup Guide

### Step 1: Create New Repository on GitHub

1. Go to https://github.com/new
2. Fill in:
   - **Repository name:** `syntaxisbio-dev.github.io`
   - **Description:** "Development version of SyntaxisBio website"
   - **Visibility:** Private (recommended) or Public
   - **Do NOT initialize with README, .gitignore, or license** (we'll copy from main repo)
3. Click "Create repository"

### Step 2: Clone and Set Up Dev Repo (I'll help with this)

After you create the repo, we'll:
- Clone the new dev repo
- Copy all files from production repo
- Push to GitHub
- Enable GitHub Pages

### Step 3: Enable GitHub Pages for Dev Repo

1. Go to https://github.com/syntaxisbio/syntaxisbio-dev.github.io
2. Click "Settings" tab
3. Scroll to "Pages" in left sidebar
4. Under "Source":
   - Branch: `main`
   - Folder: `/ (root)`
   - Click "Save"
5. Wait 1-2 minutes for site to build

## Your URLs

- **Production:** https://syntaxisbio.github.io
- **Development:** https://syntaxisbio-dev.github.io

## Workflow: Syncing Changes Between Repos

### Making Changes in Dev

```bash
# Work in dev repo
cd ~/Documents/GitHub/syntaxisbio-dev.github.io

# Make changes to files
# Test locally

# Commit and push
git add .
git commit -m "Description of changes"
git push origin main

# Wait 1-2 minutes, then check https://syntaxisbio-dev.github.io
```

### Promoting Dev to Production

When dev changes are approved and ready for production:

```bash
# Copy specific files from dev to production
cd ~/Documents/GitHub/syntaxisbio.github.io

# Copy the updated file(s)
cp ../syntaxisbio-dev.github.io/index.html .
# Or copy multiple files:
cp ../syntaxisbio-dev.github.io/index.html ../syntaxisbio-dev.github.io/styles.css .

# Review the changes
git diff

# Commit and push to production
git add .
git commit -m "Promote changes from dev: [description]"
git push origin main

# Production updates at https://syntaxisbio.github.io
```

### Full Sync from Dev to Production

If you want to sync everything:

```bash
cd ~/Documents/GitHub/syntaxisbio.github.io

# Copy all HTML/CSS/JS files (but not git history)
rsync -av --exclude='.git' --exclude='.gitignore' --exclude='README.md' \
  ../syntaxisbio-dev.github.io/ .

# Review what changed
git status
git diff

# Commit and push
git add .
git commit -m "Sync from dev repository"
git push origin main
```

## Best Practices

1. **Always develop in dev repo first**
2. **Test at syntaxisbio-dev.github.io before promoting**
3. **Keep track of which changes are in dev vs production**
4. **Use descriptive commit messages**
5. **Don't make quick fixes directly in production** (unless emergency)

## Helper Scripts

Create these scripts to make syncing easier:

### sync-to-prod.sh
```bash
#!/bin/bash
# Copy from dev to production and commit

echo "Syncing from dev to production..."
cd ~/Documents/GitHub/syntaxisbio.github.io

rsync -av --exclude='.git' --exclude='.gitignore' \
  ../syntaxisbio-dev.github.io/ .

echo "Changes to be committed:"
git status

read -p "Commit and push these changes? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]
then
    git add .
    read -p "Enter commit message: " msg
    git commit -m "$msg"
    git push origin main
    echo "Pushed to production!"
else
    echo "Sync cancelled"
fi
```

Make it executable: `chmod +x sync-to-prod.sh`

## Keeping Repos in Sync

Both repos will diverge over time. Here's how to manage:

### Strategy 1: Dev is Source of Truth (Recommended)
- All changes happen in dev first
- Production is synced from dev when approved
- Never make changes directly in production repo

### Strategy 2: Occasional Sync Back
If you accidentally make changes in production:

```bash
# Copy production changes back to dev
cd ~/Documents/GitHub/syntaxisbio-dev.github.io

rsync -av --exclude='.git' --exclude='.gitignore' \
  ../syntaxisbio.github.io/ .

git add .
git commit -m "Sync from production"
git push origin main
```

## Troubleshooting

### Dev site not updating
- Check GitHub Actions tab for build status
- Wait 2-3 minutes after push
- Hard refresh browser (Cmd+Shift+R)

### Forgot which repo I'm in
```bash
pwd                    # Shows current directory
git remote -v          # Shows repository URL
```

### Made changes in wrong repo
```bash
# If uncommitted, just copy the file to correct repo
cp file.html ~/Documents/GitHub/[correct-repo]/

# If committed, cherry-pick the commit
git log                # Find commit hash
cd ~/Documents/GitHub/[correct-repo]
git cherry-pick [commit-hash]
```
