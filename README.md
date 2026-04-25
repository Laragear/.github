# .github
Organization-wide defaults

## Github Workflows

### Managing issues

```yaml
name: Issue Reproduction Check

on:
  issues:
    types: [opened, edited]

jobs:
  call-workflow:
    # Syntax: {owner}/{repo}/.github/workflows/{filename}@{ref}
    uses: Laragear/.github/.github/workflows/issue-manager.yml@main
    secrets: inherit
```

### Calling Tests

```yaml
name: Tests - PHP 8.5 - PHPUnit

concurrency:
  group: ${{ github.ref }}
  # If there is already one running, stop the previous and start the new one.
  cancel-in-progress: true

on:
  push:
    branches:
      - "*.x"
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  test:
    name: Testing under PHP 8.5 and PHPUnit
    uses: Laragear/.github/.github/workflows/test-php-8.5-phpunit.yml@main
    secrets: inherit                      # required to pass org-wide secrets.
    with:
      laravel_constraint_min: "^12.29"    # Required for Laravel Session Cache. Adjust or remove.
      expected_files: "LICENSE.md,README.md,composer.json" # Add or remove for defaults.
```
