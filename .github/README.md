# GitHub Actions & Automation

This directory contains GitHub Actions workflows and configuration for automated dependency management and continuous integration.

## Workflows

### 1. CI Pipeline (`ci.yml`)
- **Trigger**: Push to main/master branches, Pull requests
- **Purpose**: Run tests, linting, and builds for both frontend and backend
- **Matrix Testing**: Tests across multiple Node.js and Python versions
- **Security**: Runs security audits for dependencies

### 2. Dependabot Auto-Merge (`dependabot-auto-merge.yml`)
- **Trigger**: Dependabot pull requests
- **Purpose**: Automatically merge safe dependency updates after tests pass
- **Safety Features**:
  - Only merges patch/minor updates (not major versions)
  - Always merges security updates
  - Waits for all CI checks to pass
  - Provides detailed comments explaining actions

### 3. Dependabot Configuration (`dependabot.yml`)
- **Purpose**: Configure automatic dependency updates
- **Scope**: 
  - Frontend npm packages
  - Backend pip packages
  - Docker images
  - GitHub Actions
- **Schedule**: Weekly updates on Mondays

## Auto-Merge Criteria

The Dependabot auto-merge workflow will automatically merge PRs when ALL of the following conditions are met:

1. ✅ **Created by Dependabot**: Only Dependabot PRs are considered
2. ✅ **Safe Update Type**: 
   - Patch updates (e.g., 1.2.3 → 1.2.4)
   - Minor updates (e.g., 1.2.3 → 1.3.0)
   - Security updates (identified by keywords)
   - Major updates (e.g., 1.2.3 → 2.0.0) require manual review
3. ✅ **CI Checks Pass**: All tests and security checks must pass
4. ✅ **No Conflicts**: PR must be mergeable

## Security Features

- **Limited Scope**: Only processes Dependabot PRs
- **Version Analysis**: Parses version numbers to identify major changes
- **CI Dependency**: Waits for all CI checks before proceeding
- **Audit Trail**: Detailed comments on all actions taken
- **Manual Override**: Major updates always require human review

## Setup Requirements

To enable this automation in your repository:

1. **Enable Dependabot**: The `dependabot.yml` file will automatically enable Dependabot
2. **Branch Protection**: Consider setting up branch protection rules requiring CI checks
3. **Permissions**: The workflows use `GITHUB_TOKEN` with appropriate permissions

## Customization

### Adding Tests
When you add actual tests to your project, update the workflow files:

**Frontend Tests** (in `ci.yml` and `dependabot-auto-merge.yml`):
```yaml
- name: Run frontend tests
  run: |
    cd frontend
    npm test
```

**Backend Tests**:
```yaml
- name: Run backend tests
  run: |
    cd backend
    pytest
```

### Adjusting Auto-Merge Rules
To modify what gets auto-merged, edit the "Check if PR is safe to auto-merge" step in `dependabot-auto-merge.yml`.

### Changing Update Schedule
Modify the `schedule` section in `dependabot.yml` to change when updates are created.

## Monitoring

- Check the "Actions" tab in your GitHub repository to monitor workflow runs
- Dependabot PRs will include comments explaining whether they were auto-merged or why they weren't
- Failed workflows will send notifications to repository maintainers

## Troubleshooting

**Common Issues:**

1. **Auto-merge not working**: Check that branch protection rules don't conflict
2. **CI checks failing**: Review the CI workflow logs for specific errors
3. **Permissions errors**: Ensure `GITHUB_TOKEN` has sufficient permissions

**Getting Help:**
- Review workflow run logs in the GitHub Actions tab
- Check Dependabot logs in the "Insights" → "Dependency graph" → "Dependabot" section