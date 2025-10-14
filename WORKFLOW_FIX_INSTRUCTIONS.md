# CI/CD Workflow Fix Instructions

## Overview
This PR addresses issues with the CI/CD workflows in this repository. Due to GitHub security restrictions, workflow files (`.github/workflows/*`) require special permissions to be modified via API.

## Changes Required

### Files to DELETE:
1. `.github/workflows/npm-grunt.yml` - Uses Grunt which is not configured in this Next.js project
2. `.github/workflows/datadog-synthetics.yml` - Requires Datadog account and configuration
3. `.github/workflows/generator-generic-ossf-slsa3-publish.yml` - Not applicable for this project

### Files to CREATE:

#### `.github/workflows/nextjs-ci.yml`
```yaml
name: Next.js CI

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18.x, 20.x]
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm ci
      - run: npm run lint --if-present
      - run: npm run build
      - run: npm test --if-present
```

## How to Apply These Changes

### Option 1: Via GitHub Web UI (Recommended)
1. Navigate to `.github/workflows/` directory
2. Delete the three files listed above
3. Create new file `nextjs-ci.yml` with the content provided
4. Commit changes to the `fix/ci-workflows-api` branch

### Option 2: Via Git Command Line
```bash
git checkout fix/ci-workflows-api
rm .github/workflows/npm-grunt.yml
rm .github/workflows/datadog-synthetics.yml
rm .github/workflows/generator-generic-ossf-slsa3-publish.yml
# Create nextjs-ci.yml with content above
git add .github/workflows/
git commit -m "Fix CI/CD: Remove broken workflows and add proper Next.js CI"
git push origin fix/ci-workflows-api
```

## Why These Changes?

### Problem with Current Workflows:
- **npm-grunt.yml**: Fails because Grunt is not installed or configured
- **datadog-synthetics.yml**: Fails because Datadog integration is not set up
- **generator-generic-ossf-slsa3-publish.yml**: Not relevant for this Next.js application

### Solution - New Next.js CI:
- ✅ Proper Node.js version matrix (18.x, 20.x)
- ✅ npm ci for reliable dependency installation
- ✅ Lint checks (if configured)
- ✅ Build verification
- ✅ Test execution (if tests exist)

## Expected Results

After applying these changes:
- ✅ CI will run on every push to main/master
- ✅ CI will run on every pull request
- ✅ Build failures will be caught early
- ✅ No more failed workflow runs from misconfigured actions

## Need Help?

If you need the AbacusAI bot to automatically apply these changes, please:
1. Go to https://github.com/apps/abacusai/installations/select_target
2. Add "Workflows: Read and write" permission
3. Re-run the automation
