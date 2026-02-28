# Renaming Dev Repository for Cleaner URL

## Current Situation
- Repository: `syntaxisbio-dev.github.io`
- URL: http://www.syntaxisbio.com/syntaxisbio-dev.github.io/

## Recommended: Rename to `dev`
- Repository: `dev`
- URL: http://www.syntaxisbio.com/dev/

Much cleaner!

## How to Rename

1. Go to: https://github.com/syntaXisbio/syntaxisbio-dev.github.io/settings

2. At the top, under "Repository name", change:
   - From: `syntaxisbio-dev.github.io`
   - To: `dev`

3. Click "Rename"

4. Update your local repository remote:
   ```bash
   cd ~/Documents/GitHub/syntaxisbio-dev.github.io
   git remote set-url origin https://github.com/syntaXisbio/dev.git
   ```

5. Optionally rename the local folder:
   ```bash
   cd ~/Documents/GitHub
   mv syntaxisbio-dev.github.io dev
   ```

6. Your dev site will then be at: **http://www.syntaxisbio.com/dev/**

## Alternative: Keep Current Name

If you prefer to keep the current name, the site works fine at:
http://www.syntaxisbio.com/syntaxisbio-dev.github.io/

Just a bit longer in the URL.
