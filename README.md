# Homebrew Tap for Billing Dashboard

This is the Homebrew tap repository for the Billing Intelligence Dashboard project.

## Installation

```bash
# Tap this repository
brew tap joshuascottpaul/billing-dashboard-project-homebrew-tap

# Install billing-dashboard
brew install billing-dashboard
```

## Usage

After installation, use the `billing-dashboard` CLI:

```bash
# Run analytics pipeline
billing-dashboard run

# Start dashboard server (default port 8000)
billing-dashboard serve

# Start dashboard server on custom port
billing-dashboard serve --port 3000

# Generate TASKS.md from tasks.yaml
billing-dashboard generate

# Run E2E tests
billing-dashboard test

# Show version
billing-dashboard version

# Show help
billing-dashboard help
```

## Uninstall

```bash
brew uninstall billing-dashboard
brew untap joshuascottpaul/billing-dashboard-project-homebrew-tap
```

## Requirements

- macOS or Linux with Homebrew
- Python 3.14 (installed automatically)
- Node.js (installed automatically)
- DuckDB (installed automatically)

## Troubleshooting

### Installation fails

Try installing dependencies first:
```bash
brew install python@3.14 node duckdb
brew install billing-dashboard
```

### Playwright browsers not installed

Run post-install manually:
```bash
brew postinstall billing-dashboard
```

### Dashboard won't start

Check if port 8000 is in use:
```bash
lsof -i :8000
billing-dashboard serve --port 3000  # Use different port
```

## Links

- Main project: https://github.com/joshuascottpaul/billing-dashboard-project
- Report issues: https://github.com/joshuascottpaul/billing-dashboard-project/issues
