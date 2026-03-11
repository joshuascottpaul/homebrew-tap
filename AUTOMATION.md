# Homebrew Automation Setup

## Automated Formula Updates

The Homebrew tap includes GitHub Actions workflows that automatically update the formula when a new release is published.

## How It Works

1. **Release Published** → A new tag/release is created in `billing-dashboard-project`
2. **Webhook Triggered** → The main repo's workflow sends a repository_dispatch event to the tap repo
3. **Formula Updated** → The tap repo workflow:
   - Downloads the new release tarball
   - Calculates the SHA256
   - Updates the formula URL and hash
   - Commits and pushes the changes

## Setup Required

### Step 1: Create Personal Access Token

1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scopes:
   - `repo` (Full control of private repositories)
4. Generate and copy the token

### Step 2: Add Token to Tap Repo Secrets

1. Go to https://github.com/joshuascottpaul/billing-dashboard-project-homebrew-tap/settings/secrets/actions
2. Click "New repository secret"
3. Add:
   - Name: `HOMEBREW_TAP_TOKEN`
   - Value: `<paste your token>`

### Step 3: Add Token to Main Repo Secrets

1. Go to https://github.com/joshuascottpaul/billing-dashboard-project/settings/secrets/actions
2. Click "New repository secret"
3. Add:
   - Name: `HOMEBREW_TAP_TOKEN`
   - Value: `<same token>`

### Step 4: Enable Workflow Permissions

In the tap repo:
1. Go to https://github.com/joshuascottpaul/billing-dashboard-project-homebrew-tap/settings/actions
2. Under "Workflow permissions", select "Read and write permissions"
3. Check "Allow GitHub Actions to create and approve pull requests"
4. Click "Save"

## Manual Update

If automation fails, update manually:

```bash
# In billing-dashboard-project-homebrew-tap repo
cd /path/to/billing-dashboard-project-homebrew-tap

# Get new version SHA256
curl -sL "https://github.com/joshuascottpaul/billing-dashboard-project/archive/refs/tags/vX.Y.Z.tar.gz" | shasum -a 256

# Edit Formula/billing-dashboard.rb with new version and SHA256
# Then commit and push
git add Formula/billing-dashboard.rb
git commit -m "Update billing-dashboard to vX.Y.Z"
git push
```

## Testing Installation

After updating the formula, test installation:

```bash
# Uninstall if already installed
brew uninstall billing-dashboard

# Update tap
brew update

# Install
brew install billing-dashboard

# Verify
billing-dashboard version
```

## Troubleshooting

### 403 Permission Denied

The GITHUB_TOKEN doesn't have write permissions. Either:
1. Enable "Read and write permissions" in repo settings (see Step 4)
2. Use a Personal Access Token instead

### Formula SHA256 Mismatch

If the tarball contents change without version bump:
```bash
# Recalculate SHA256
curl -sL "https://github.com/joshuascottpaul/billing-dashboard-project/archive/refs/tags/v0.1.0.tar.gz" | shasum -a 256

# Update formula with new hash
```

### Workflow Not Triggering

Check:
1. Release was published (not just draft)
2. Token has `repo` scope
3. Workflow is enabled in Actions tab
