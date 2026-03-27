# CI/CD Fixes Required

## Issue
The current repository has 3 broken workflow files that are causing CI/CD failures:
- `.github/workflows/datadog-synthetics.yml`
- `.github/workflows/generator-generic-ossf-slsa3-publish.yml`
- `.github/workflows/npm-grunt.yml`

## Solution

### Step 1: Delete Broken Workflows
Remove the following files:
```bash
rm .github/workflows/datadog-synthetics.yml
rm .github/workflows/generator-generic-ossf-slsa3-publish.yml
rm .github/workflows/npm-grunt.yml
```

### Step 2: Add New Next.js CI Workflow
Create a new file `.github/workflows/nextjs-ci.yml` with the content from `workflow-templates/nextjs-ci.yml`

## Why These Changes Are Needed
- The broken workflows reference non-existent configurations or unsupported actions
- The new Next.js CI workflow provides proper testing on Node.js 18.x and 20.x
- Includes linting, building, and testing steps appropriate for a Next.js project

## Note on GitHub App Permissions
⚠️ **Important**: To apply these workflow changes automatically, the AbacusAI GitHub App needs the `workflows` permission. 

To grant this permission:
1. Go to https://github.com/apps/abacusai/installations/select_target
2. Select your account/organization
3. Grant "Read and write" access to "Workflows"

Without this permission, workflow files must be modified manually.
