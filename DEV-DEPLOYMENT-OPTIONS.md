# Development Environment Deployment Options

## The Challenge
You want both production and development versions of the site accessible via URLs for team review.

## Option 1: Separate Dev Repository (Simplest for GitHub Pages)

**Setup:**
- `syntaxisbio.github.io` → Production (main branch)
- `syntaxisbio-dev.github.io` → Development (separate repo)

**Pros:**
- Clean separation
- Both have public URLs
- Simple GitHub Pages configuration
- No custom workflows needed

**Cons:**
- Must maintain two repositories
- Files need to be synced between repos

**How to set up:**
```bash
# Create new repo on GitHub: syntaxisbio-dev
# Clone it
cd ~/Documents/GitHub
git clone https://github.com/syntaxisbio/syntaxisbio-dev.github.io.git
cd syntaxisbio-dev.github.io

# Copy files from main repo
cp -r ../syntaxisbio.github.io/* .

# Commit and push
git add .
git commit -m "Initial dev site"
git push

# Enable GitHub Pages
# Go to repo Settings → Pages → Source: main branch → Save
```

**URLs:**
- Production: https://syntaxisbio.github.io
- Development: https://syntaxisbio-dev.github.io

---

## Option 2: Netlify (Recommended - Best Developer Experience)

**Setup:**
- Connect your GitHub repo to Netlify (free)
- Automatic deployments for all branches
- Each branch/PR gets its own preview URL

**Pros:**
- Free tier is generous
- Automatic deploy previews for every branch and PR
- Better than GitHub Pages for staging
- Custom domains easy to set up
- Instant rollbacks
- Built-in forms (for your contact form)
- Better performance/CDN

**Cons:**
- Adds external dependency
- Need to create Netlify account

**How to set up:**

1. Go to https://netlify.com and sign up (free)
2. Click "Add new site" → "Import an existing project"
3. Connect to GitHub → Select `syntaxisbio.github.io`
4. Configure:
   - Branch to deploy: `main`
   - Build command: (leave empty for static site)
   - Publish directory: `/` (root)
5. Click "Deploy"

**Netlify will automatically:**
- Deploy `main` branch to production URL (e.g., syntaxisbio.netlify.app)
- Deploy `dev` branch to https://dev--syntaxisbio.netlify.app
- Create preview URLs for every pull request

**Custom domain:**
You can later point syntaxisbio.com to Netlify production and dev.syntaxisbio.com to dev branch.

**URLs:**
- Production: https://syntaxisbio.netlify.app (or your custom domain)
- Dev branch: https://dev--syntaxisbio.netlify.app
- Any PR: https://deploy-preview-[PR#]--syntaxisbio.netlify.app

---

## Option 3: GitHub Actions + GitHub Pages Subdirectory

**Setup:**
- Production: main branch → https://syntaxisbio.github.io
- Development: dev branch → https://syntaxisbio.github.io/dev

**Pros:**
- Everything stays in GitHub
- Single repository
- Both environments accessible

**Cons:**
- Requires custom GitHub Actions workflow
- More complex setup
- Dev site at /dev path (not root), may cause relative link issues

**How to set up:**

Would require creating a custom GitHub Actions workflow to:
1. Build from dev branch
2. Copy files to /dev subdirectory
3. Publish to gh-pages branch

This is more complex and I'd need to create the workflow file for you.

---

## Option 4: Vercel (Similar to Netlify)

**Setup:**
- Similar to Netlify
- Connect GitHub repo
- Automatic preview deployments

**Pros:**
- Free tier
- Excellent performance
- Great developer experience
- Preview URLs for every push

**Cons:**
- Another external service
- Similar to Netlify (choose one or the other)

---

## My Recommendation

**Best choice: Option 2 (Netlify)**

**Why:**
1. **Zero configuration** - Works immediately with your static site
2. **Branch previews** - Every branch gets a URL automatically
3. **Pull request previews** - Team can review changes before merging
4. **Free** - Free tier is more than enough for your needs
5. **Better than GitHub Pages** - Faster deploys, better performance
6. **Forms work better** - Netlify has built-in form handling (you're using Formspree now, but Netlify Forms are free)
7. **Easy rollbacks** - One-click rollback to any previous deploy

**Workflow with Netlify:**
```bash
# Work on dev branch
git checkout dev
# Make changes
git add .
git commit -m "Update services pricing"
git push origin dev

# Dev branch automatically deploys to:
# https://dev--syntaxisbio.netlify.app

# Team reviews at dev URL
# When approved, merge to main:
git checkout main
git merge dev
git push origin main

# Production automatically deploys to:
# https://syntaxisbio.netlify.app
```

**Alternative if you want to stay 100% in GitHub: Option 1 (Separate repo)**

It's simple, works with GitHub Pages, and gives you two clean URLs. Just a bit more manual syncing between repos.

---

## Quick Start: Setting Up Netlify (5 minutes)

1. **Sign up:** Go to https://app.netlify.com/signup (use GitHub login)

2. **Import project:**
   - Click "Add new site" → "Import an existing project"
   - Choose "GitHub"
   - Authorize Netlify to access your repos
   - Select `syntaxisbio/syntaxisbio.github.io`

3. **Configure deploy settings:**
   - Branch to deploy: `main`
   - Build command: (leave blank)
   - Publish directory: `/` or `.`
   - Click "Deploy site"

4. **Enable branch deploys:**
   - Go to Site settings → Build & deploy → Continuous Deployment
   - Under "Branch deploys" click "Edit settings"
   - Select "Let me add individual branches"
   - Add branch: `dev`
   - Save

5. **Done!** You now have:
   - Production: https://[random-name].netlify.app (you can customize this)
   - Dev: https://dev--[random-name].netlify.app

6. **Optional - Custom domain:**
   - Site settings → Domain management → Add custom domain
   - Follow instructions to point syntaxisbio.com

---

## Comparison Table

| Feature | GitHub Pages (2 repos) | Netlify | GitHub Actions + Subdir |
|---------|----------------------|---------|------------------------|
| Setup complexity | Low | Very Low | High |
| Cost | Free | Free | Free |
| Production URL | syntaxisbio.github.io | Custom/subdomain | syntaxisbio.github.io |
| Dev URL | syntaxisbio-dev.github.io | dev--site.netlify.app | syntaxisbio.github.io/dev |
| PR previews | No | Yes (automatic) | Possible (complex) |
| Deploy speed | 1-2 min | 10-30 sec | 1-2 min |
| Rollbacks | Manual (git revert) | One-click | Manual |
| Form handling | Need external (Formspree) | Built-in free | Need external |
| CDN performance | Good | Excellent | Good |

---

## What Should You Do?

**For minimal change:** Use separate dev repo (Option 1)
**For best experience:** Use Netlify (Option 2) ← **I recommend this**

Want me to help you set up either option?
