# Syncing to Community Apps (selfhosters PR)

This document describes how to sync changes from this repo to the selfhosters CA PR.

## Two-Repo Setup

| Repo | Purpose | URL |
|------|---------|-----|
| **openclaw-unraid** | Personal template repo (active development) | https://github.com/jdhill777/openclaw-unraid |
| **unRAID-CA-templates** | Fork of selfhosters (for PR submission) | https://github.com/jdhill777/unRAID-CA-templates |

**PR #626:** https://github.com/selfhosters/unRAID-CA-templates/pull/626

## When to Sync

Sync to the CA PR when:
- Major features are stable and tested
- Bug fixes that affect new users
- Before CA approval (to ensure reviewers see latest version)

No need to sync for every small change — batch updates are fine.

## What Changes Between Repos

Only **icon/screenshot URLs** differ:

| Element | openclaw-unraid | selfhosters PR |
|---------|-----------------|----------------|
| Icon | `https://raw.githubusercontent.com/jdhill777/openclaw-unraid/master/openclaw-icon.png` | `https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/openclaw.png` |
| Screenshot | `https://raw.githubusercontent.com/jdhill777/openclaw-unraid/master/screenshot.png` | `https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/openclaw-screenshot.png` |

Everything else (donation link, features, config) stays the same.

## Sync Process

### 1. Prep the template

```bash
# Start from the openclaw-unraid template
cp /path/to/openclaw-unraid/openclaw.xml /tmp/openclaw-ca.xml

# Update icon URLs (sed or manual edit)
sed -i 's|jdhill777/openclaw-unraid/master/openclaw-icon.png|selfhosters/unRAID-CA-templates/master/templates/img/openclaw.png|g' /tmp/openclaw-ca.xml
sed -i 's|jdhill777/openclaw-unraid/master/screenshot.png|selfhosters/unRAID-CA-templates/master/templates/img/openclaw-screenshot.png|g' /tmp/openclaw-ca.xml
```

### 2. Update the fork

```bash
cd /home/node/clawd/projects/selfhosters-pr

# Make sure fork is up to date with upstream
git fetch upstream
git checkout master
git merge upstream/master
git push origin master

# Switch to PR branch
git checkout add-openclaw-template

# Rebase on latest master (if needed)
git rebase master

# Copy the updated template
cp /tmp/openclaw-ca.xml templates/openclaw.xml

# Commit and push
git add templates/openclaw.xml
git commit -m "feat: <description of changes>"
git push origin add-openclaw-template
```

### 3. Verify

- Check PR #626 on GitHub — it should show the new commits
- Review the diff to make sure icon URLs are correct

## Image Files

If you update the icon or screenshot, you also need to update them in the selfhosters fork:

```bash
# Copy images to the fork
cp openclaw-icon.png /home/node/clawd/projects/selfhosters-pr/templates/img/openclaw.png
cp screenshot.png /home/node/clawd/projects/selfhosters-pr/templates/img/openclaw-screenshot.png

# Add to commit
git add templates/img/openclaw.png templates/img/openclaw-screenshot.png
```

## After CA Approval

Once the PR is merged:
- Users will get the template from Community Apps automatically
- You can continue maintaining `openclaw-unraid` for:
  - Users who want bleeding-edge updates
  - Pre-CA-approval installs
  - Documentation and README

For future updates to the CA template, open a new PR to selfhosters.

## Local Paths Reference

- **openclaw-unraid repo:** `/home/node/clawd/projects/openclaw-unraid-template/`
- **selfhosters fork:** `/home/node/clawd/projects/selfhosters-pr/`
- **Template in fork:** `/home/node/clawd/projects/selfhosters-pr/templates/openclaw.xml`
- **Images in fork:** `/home/node/clawd/projects/selfhosters-pr/templates/img/`
