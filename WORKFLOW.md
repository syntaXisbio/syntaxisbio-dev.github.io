# SyntaxisBio Development Workflow

## Your Two Sites

**Production**
- URL: https://www.syntaxisbio.com
- Repository: https://github.com/syntaXisbio/syntaxisbio.github.io
- Local path: `~/Documents/GitHub/syntaxisbio.github.io`

**Development**
- URL: https://www.syntaxisbio.com/dev/
- Repository: https://github.com/syntaXisbio/dev
- Local path: `~/Documents/GitHub/dev`
- Protected from search engines: ✓ (robots.txt)

## Daily Workflow

### Working on Dev Site

```bash
# Navigate to dev
cd ~/Documents/GitHub/dev

# Make your changes to files (edit index.html, CSS, etc.)

# Test locally
open index.html
# Or use Python server:
python3 -m http.server 8000
# Visit http://localhost:8000

# When satisfied, commit and push
git add .
git commit -m "Description of what you changed"
git push origin main

# Wait 1-2 minutes, then check:
# https://www.syntaxisbio.com/dev/
```

### Promoting Dev to Production

When changes are tested and approved on dev, promote them to production:

```bash
# Navigate to production
cd ~/Documents/GitHub/syntaxisbio.github.io

# Copy updated file(s) from dev
cp ../dev/index.html .

# Or copy multiple files
cp ../dev/index.html ../dev/styles.css .

# Review what changed
git diff

# Commit and push to production
git add .
git commit -m "Promote from dev: description of changes"
git push origin main

# Production updates at https://www.syntaxisbio.com
```

### Full Sync from Dev to Production

To sync everything from dev to production:

```bash
cd ~/Documents/GitHub/syntaxisbio.github.io

# Sync all files (except git files)
rsync -av --exclude='.git' --exclude='.gitignore' --exclude='README.md' \
  ../dev/ .

# Review changes
git status
git diff

# Commit and push
git add .
git commit -m "Full sync from dev"
git push origin main
```

## Quick Reference Commands

```bash
# Check which repo you're in
pwd
git remote -v

# See what branch you're on
git branch

# See uncommitted changes
git status

# See what changed in files
git diff

# Discard uncommitted changes (careful!)
git checkout -- filename.html

# Pull latest changes from GitHub
git pull origin main
```

## Best Practices

1. **Always develop in dev first** - Never make changes directly in production
2. **Test locally** - Open files in browser before pushing
3. **Test on dev URL** - Share https://www.syntaxisbio.com/dev/ with team for review
4. **Descriptive commits** - Write clear commit messages
5. **Small changes** - Commit frequently with focused changes

## Tips

- Dev site updates appear in 1-2 minutes after push
- Hard refresh browser if changes don't appear: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows)
- Dev site won't appear in Google search (robots.txt blocks it)
- Share dev URL only with your team for internal review

## File Locations

```
~/Documents/GitHub/
├── syntaxisbio.github.io/    # Production
│   ├── index.html
│   ├── images/
│   └── ...
└── dev/                       # Development
    ├── index.html
    ├── images/
    └── ...
```

## Common Tasks

### Update homepage content
```bash
cd ~/Documents/GitHub/dev
# Edit index.html
git add index.html
git commit -m "Update homepage content"
git push
```

### Add new image
```bash
cd ~/Documents/GitHub/dev
# Copy image to images/ folder
git add images/new-image.png
git commit -m "Add new team member photo"
git push
```

### Emergency production fix
```bash
# If you must fix production directly (rare)
cd ~/Documents/GitHub/syntaxisbio.github.io
# Make minimal fix
git add .
git commit -m "Hotfix: describe emergency fix"
git push

# Then sync fix back to dev
cd ~/Documents/GitHub/dev
cp ../syntaxisbio.github.io/index.html .
git add index.html
git commit -m "Sync hotfix from production"
git push
```
