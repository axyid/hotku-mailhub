# HOTKU MailHub Deployment Guide

## 📦 NPM Publishing

### Prerequisites
1. Create NPM account: https://www.npmjs.com/signup
2. Login to NPM: `npm login`
3. Verify account: `npm whoami`

### Publishing Steps

```bash
# 1. Install dependencies
npm install

# 2. Test the package
npm test

# 3. Check package contents
npm pack --dry-run

# 4. Publish to NPM
npm publish

# 5. Verify publication
npm view @masanto/hotku-mailhub
```

### Version Management

```bash
# Patch version (1.0.0 -> 1.0.1)
npm version patch

# Minor version (1.0.0 -> 1.1.0)
npm version minor

# Major version (1.0.0 -> 2.0.0)
npm version major

# Custom version
npm version 1.2.3
```

## 🐙 GitHub Publishing

### Initial Setup

```bash
# 1. Initialize git repository
git init

# 2. Add remote repository
git remote add origin https://github.com/masanto/hotku-mailhub.git

# 3. Add all files
git add .

# 4. Initial commit
git commit -m "Initial commit: HOTKU MailHub v1.0.0"

# 5. Push to GitHub
git branch -M main
git push -u origin main
```

### Creating Release

```bash
# 1. Create and push tag
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0

# 2. Create GitHub release (via web interface)
# Go to: https://github.com/masanto/hotku-mailhub/releases/new
```

### Continuous Updates

```bash
# 1. Make changes
git add .
git commit -m "feat: add new feature"

# 2. Update version
npm version patch

# 3. Push changes and tags
git push && git push --tags

# 4. Publish to NPM
npm publish
```

## 🔧 Automated Deployment

### GitHub Actions (Optional)

Create `.github/workflows/publish.yml`:

```yaml
name: Publish to NPM

on:
  push:
    tags:
      - 'v*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
          registry-url: 'https://registry.npmjs.org'
      - run: npm ci
      - run: npm test
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## 📋 Pre-publish Checklist

- [ ] Update version in package.json
- [ ] Update CHANGELOG.md
- [ ] Run tests: `npm test`
- [ ] Check package contents: `npm pack --dry-run`
- [ ] Verify README.md is up to date
- [ ] Check all files are included in package.json "files" array
- [ ] Test CLI globally: `npm install -g . && hotku-mailhub`
- [ ] Commit all changes
- [ ] Create git tag
- [ ] Push to GitHub
- [ ] Publish to NPM

## 🌐 Post-publish Tasks

1. **Update documentation**
2. **Announce release** on social media
3. **Update related projects**
4. **Monitor for issues**
5. **Respond to user feedback**

## 🔍 Verification Commands

```bash
# Check if package is published
npm view @masanto/hotku-mailhub

# Install and test globally
npm install -g @masanto/hotku-mailhub
hotku-mailhub --help

# Test as dependency
mkdir test-project && cd test-project
npm init -y
npm install @masanto/hotku-mailhub
node -e "console.log(require('@masanto/hotku-mailhub'))"
```
