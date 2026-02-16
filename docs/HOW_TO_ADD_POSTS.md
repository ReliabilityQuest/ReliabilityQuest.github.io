# How to Add a New Post and Deploy to reliability.quest

This guide explains how to create a new blog post and deploy it to your GitHub Pages site at reliability.quest.

## Quick Start

```bash
# 1. Create a new post file
cd /home/conger/Documents/Projects/reliabilityquest/ReliabilityQuest.github.io

# 2. Create file with format: YYYY-MM-DD-your-post-title.md
nano _posts/2025-11-03-your-post-title.md

# 3. Build the site
bundle exec jekyll build

# 4. Verify CNAME file exists
cat docs/CNAME

# 5. Commit and push
git add .
git commit -m "Add new post: Your Post Title"
git push origin Master
```

## Detailed Instructions

### Step 1: Create Your Post File

Create a new file in the `_posts/` directory with this naming format:

```
YYYY-MM-DD-your-post-title.md
```

**Example:** `2025-11-03-understanding-system-reliability.md`

### Step 2: Add Front Matter

Every post must start with YAML front matter. Here's the minimum required:

```yaml
---
layout: post
title: "Your Post Title"
date: 2025-11-03 10:00:00
categories: post
---
```

**Required fields:**
- `layout: post` - Uses the post layout template
- `title:` - Your post title (use quotes if it contains special characters)
- `date:` - Publication date and time (format: YYYY-MM-DD HH:MM:SS)
- `categories: post` - Categorizes as a blog post

### Step 3: Write Your Content

After the front matter, write your content using **GitHub-Flavored Markdown**.

**Basic example:**
```markdown
---
layout: post
title: "Understanding System Reliability"
date: 2025-11-03 10:00:00
categories: post
---
This is my introduction paragraph.
<!--more-->

## Main Content

Here's the main content of my post...
```

**Note:** The `<!--more-->` tag creates an excerpt separator - everything before it appears in post previews.

### Step 4: Use Custom Liquid Tags (Optional)

This site uses the Tufte-Jekyll theme with custom tags for enhanced typography:

#### New Thought (Small Caps)
```liquid
{% newthought "This will appear in small caps" %} followed by regular text.
```

#### Sidenote (Numbered Note in Margin)
```liquid
Here's some text{% sidenote "sn-1" "This appears as a numbered note in the margin" %} continuing.
```

#### Margin Note (Unnumbered Note in Margin)
```liquid
More text{% marginnote "mn-1" "This appears in the margin without a number" %} and more.
```

#### Full Width Image
```liquid
{% fullwidth "assets/img/your-image.png" "Caption for the image" %}
```

#### Main Column Image
```liquid
{% maincolumn "assets/img/your-image.png" "Caption for the image" %}
```

#### Margin Figure
```liquid
{% marginfigure "mf-1" "assets/img/your-image.png" "Caption text" %}
```

**Important:** Each tag with an ID (sidenote, marginnote, marginfigure) needs a unique ID per page.

### Step 5: Build the Site

From the repository root, run:

```bash
bundle exec jekyll build
```

This builds the static site into the `docs/` folder. The build process:
- Generates HTML from your Markdown
- Processes all Liquid tags
- Copies static assets
- **Automatically copies CNAME file to docs/ folder**

**Expected output:**
```
Configuration file: /path/to/_config.yml
            Source: .
       Destination: ./docs
      Generating... done in X.XXX seconds.
```

### Step 6: Verify CNAME File

**Critical Step:** Ensure the CNAME file exists in the docs folder:

```bash
cat docs/CNAME
```

**Expected output:** `reliability.quest`

If missing, the site will break! Ensure:
1. `/CNAME` exists in root (should contain `reliability.quest`)
2. Run `bundle exec jekyll build` again to copy it to docs/

### Step 7: Commit Your Changes

```bash
git add .
git commit -m "Add new post: Your Post Title"
```

**What gets committed:**
- Your new post file in `_posts/`
- The entire `docs/` folder with generated site
- Any images or assets you added

### Step 8: Deploy to GitHub Pages

```bash
git push origin Master
```

GitHub Pages will automatically serve your updated site from the `docs/` folder within a few moments.

## Deployment Architecture

### How It Works

**GitHub Pages Configuration:**
- **Source Branch:** Master
- **Source Folder:** `/docs`
- **Custom Domain:** reliability.quest (via CNAME)

**Why Build Locally?**

This site uses custom Jekyll plugins that GitHub Pages doesn't support. Therefore:
1. We build the site **locally** using `bundle exec jekyll build`
2. We commit the **generated files** in `docs/`
3. GitHub Pages serves the **pre-built static files**

### File Structure

```
ReliabilityQuest.github.io/
├── _posts/              # Your markdown posts (source)
├── _layouts/            # Page templates
├── _includes/           # Reusable components
├── assets/              # Images, CSS, fonts
├── docs/                # Generated site (deployed) ⚠️
│   ├── CNAME            # Must exist!
│   └── [generated files]
├── CNAME                # Source (copied to docs/)
├── _config.yml          # Jekyll configuration
└── Gemfile              # Ruby dependencies
```

## Troubleshooting

### Site Not Updating After Push

1. Check GitHub repository to ensure docs/ folder was committed
2. Verify CNAME file exists: `ls -la docs/CNAME`
3. Check GitHub Pages settings: Repository → Settings → Pages
4. Wait 1-2 minutes for GitHub to rebuild (they cache)

### Custom Domain Not Working

```bash
# Verify CNAME in both locations
cat CNAME
cat docs/CNAME

# If missing, rebuild
bundle exec jekyll build

# Verify and commit
git add docs/CNAME
git commit -m "Fix: Restore CNAME file"
git push origin Master
```

### Build Errors

```bash
# Update dependencies
bundle install

# Try building with verbose output
bundle exec jekyll build --verbose

# Check for syntax errors in your post's front matter
```

### Post Not Appearing

- Ensure filename format is correct: `YYYY-MM-DD-title.md`
- Check date isn't in the future
- Verify front matter has `layout: post`
- Rebuild and push again

## Tips and Best Practices

1. **Test locally first:** Run `bundle exec jekyll serve` and preview at `http://localhost:4000`
2. **Image paths:** Use relative paths without leading slash: `assets/img/photo.png`
3. **Commit often:** Commit after each post or significant change
4. **Descriptive commits:** Use clear commit messages like "Add post: Title" or "Update post: Fix typo"
5. **Preview before pushing:** Always preview your built site locally first
6. **Keep IDs unique:** When using sidenotes/marginfigures, use descriptive unique IDs per page

## Example: Complete Workflow

```bash
# Navigate to repository
cd ~/Documents/Projects/reliabilityquest/ReliabilityQuest.github.io

# Create new post
cat > _posts/2025-11-03-five-nines-reliability.md << 'EOF'
---
layout: post
title: "Achieving Five Nines Reliability"
date: 2025-11-03 14:30:00
categories: post
---
The quest for 99.999% uptime is challenging but achievable.
<!--more-->

{% newthought "Five nines reliability" %} means your system is available 99.999% of the time. That's only 5.26 minutes of downtime per year{% sidenote "sn-calc" "Calculated as: 365.25 days × 24 hours × 60 minutes × 0.00001 = 5.26 minutes" %}.

## Key Strategies

1. **Redundancy at every layer**
2. **Automated failover**
3. **Comprehensive monitoring**
4. **Regular disaster recovery drills**

{% marginfigure "mf-arch" "assets/img/ha-architecture.png" "High availability architecture diagram" %}

The path to five nines requires both technical excellence and operational discipline.
EOF

# Build the site
bundle exec jekyll build

# Verify CNAME
cat docs/CNAME

# Preview locally (optional)
bundle exec jekyll serve

# Commit and deploy
git add .
git commit -m "Add post: Achieving Five Nines Reliability"
git push origin Master

# Done! Check https://reliability.quest in 1-2 minutes
```

## Resources

- **Jekyll Documentation:** https://jekyllrb.com/docs/
- **Markdown Guide:** https://www.markdownguide.org/
- **Tufte-CSS Reference:** https://edwardtufte.github.io/tufte-css/
- **Site Theme Details:** See README.md in repository root

## Questions?

If you encounter issues, check:
1. This guide's troubleshooting section
2. The README.md file for theme-specific details
3. GitHub Actions tab (if configured) for build errors
