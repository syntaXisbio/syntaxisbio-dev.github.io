# Development Workflow for SyntaxisBio Website

## Branch Strategy

### Production Branch: `main`
- **Purpose:** Live production site
- **URL:** https://syntaxisbio.github.io
- **Rule:** Only merge tested, approved changes from `dev`
- **Protection:** Consider enabling branch protection rules

### Development Branch: `dev`
- **Purpose:** Active development and testing
- **Testing:** Test locally before merging to `main`
- **Merge:** Only merge to `main` when changes are verified and approved

## Workflow Steps

### 1. Making Changes

```bash
# Start from dev branch
git checkout dev
git pull origin dev

# Make your changes to files
# Test locally by opening index.html in browser

# When satisfied, commit changes
git add .
git commit -m "Description of changes"
git push origin dev
```

### 2. Testing Locally

Before merging to production, test your changes:

```bash
# Option A: Open file directly
# Navigate to your project folder and open index.html in browser

# Option B: Use Python simple server (recommended)
cd /Users/alexp/Documents/GitHub/syntaxisbio.github.io
python3 -m http.server 8000
# Then visit http://localhost:8000 in your browser

# Option C: Use VS Code Live Server extension
# Right-click index.html → "Open with Live Server"
```

Test thoroughly:
- [ ] All navigation links work
- [ ] Forms function correctly
- [ ] Images load properly
- [ ] Mobile responsive layout
- [ ] No console errors (check browser developer tools)

### 3. Merging to Production

Once dev branch is tested and approved:

```bash
# Switch to main branch
git checkout main
git pull origin main

# Merge dev into main
git merge dev

# Push to production
git push origin main
```

GitHub Pages will automatically rebuild and deploy within 1-2 minutes.

### 4. Verify Production Deploy

After pushing to main:
- Wait 1-2 minutes for GitHub Pages to rebuild
- Visit https://syntaxisbio.github.io
- Verify changes are live and working correctly
- Check for any issues that didn't appear in local testing

## Quick Reference Commands

```bash
# Create dev branch (first time only)
git checkout -b dev
git push -u origin dev

# Switch between branches
git checkout dev          # Switch to development
git checkout main         # Switch to production

# See which branch you're on
git branch

# See status of changes
git status

# Discard local changes (careful!)
git checkout -- filename.html
```

## Best Practices

1. **Never commit directly to main** - Always develop on `dev` first
2. **Test locally before merging** - Catch issues before they go live
3. **Make small, focused commits** - Easier to track and revert if needed
4. **Write descriptive commit messages** - Explain what and why
5. **Keep dev and main in sync** - Regularly merge main back into dev if needed

## Setting Up Branch Protection (Optional)

To prevent accidental commits to main:

1. Go to repository on GitHub
2. Settings → Branches
3. Add rule for `main` branch
4. Enable "Require pull request reviews before merging"
5. Enable "Require status checks to pass before merging"

This forces all changes to go through pull requests from dev to main.

## Troubleshooting

### Changes not appearing on live site
- Wait 2-3 minutes for GitHub Pages rebuild
- Check GitHub Actions tab for build status
- Hard refresh browser (Cmd+Shift+R on Mac, Ctrl+Shift+R on Windows)
- Clear browser cache

### Merge conflicts
```bash
# If merge conflict occurs
git status                    # See conflicted files
# Manually edit files to resolve conflicts
git add .                     # Stage resolved files
git commit -m "Resolve merge conflicts"
```

### Accidentally committed to wrong branch
```bash
# If you committed to main instead of dev
git reset --soft HEAD~1       # Undo last commit, keep changes
git stash                     # Save changes
git checkout dev              # Switch to dev
git stash pop                 # Restore changes
git commit -m "Your message"  # Commit to dev
```

## File Organization Tips

Keep development files organized:
```
syntaxisbio.github.io/
├── index.html                    # Production HTML
├── images/                       # Image assets
├── legal-templates/              # Legal document templates
├── services-content-draft.html   # Draft content (not linked in production)
├── DEV-WORKFLOW.md              # This workflow guide
└── README.md                     # Project documentation
```

Files ending in `-draft` or in `drafts/` folder won't be visible on the live site unless you link to them.
