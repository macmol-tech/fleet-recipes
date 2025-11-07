# FleetImporter Tests

This directory contains test scripts to validate that all recipes comply with the style guide requirements defined in [CONTRIBUTING.md](../CONTRIBUTING.md).

## Test Files

### `test_style_guide_compliance.py`

Validates that all recipe files comply with the style guide requirements:

- ✅ **YAML syntax** is valid (proper YAML formatting)
- ✅ **Required AutoPkg fields** present (`Description`, `Identifier`, `Input`, `Process`)
- ✅ Filename conventions (`.fleet.recipe.yaml` for combined recipes, or legacy `.fleet.direct/gitops.recipe.yaml`)
- ✅ Vendor folder structure (no spaces in folder names, proper organization)
- ✅ Identifier patterns (`com.github.fleet.<Name>` for combined, or legacy `com.github.fleet.direct/gitops.<Name>`)
- ✅ Single processor stage (FleetImporter only)
- ✅ `NAME` variable exists in all recipes
- ✅ `SELF_SERVICE` must be set to `true` in all recipes
- ✅ `AUTOMATIC_INSTALL` must be set to `false` in all recipes
- ✅ `GITOPS_MODE` variable exists and defaults to `false` in combined recipes
- ✅ `CATEGORIES` required when `SELF_SERVICE` is `true`
- ✅ Only one of `LABELS_INCLUDE_ANY` or `LABELS_EXCLUDE_ANY` can be set (mutually exclusive)
- ✅ `FLEET_GITOPS_SOFTWARE_DIR` must be set to `lib/macos/software`
- ✅ `FLEET_GITOPS_TEAM_YAML_PATH` must be set to `teams/workstations.yml`
- ✅ Categories use only supported Fleet values: `Browsers`, `Communication`, `Developer tools`, `Productivity`
- ✅ All Process arguments properly reference Input variables (`%VARIABLE%` format)

## Recipe Format Evolution

This repository now uses a **combined recipe format** that supports both direct and GitOps modes in a single file:

- **Preferred**: `.fleet.recipe.yaml` - Combined recipe supporting both modes
- **Legacy**: `.fleet.direct.recipe.yaml` and `.fleet.gitops.recipe.yaml` - Separate files for each mode

The test suite validates both formats but will warn on legacy recipes, encouraging migration to the combined format.

## Running Tests Locally

### Prerequisites

```bash
python3 -m pip install PyYAML
```

### Run Style Guide Compliance Test

```bash
python3 tests/test_style_guide_compliance.py
```

### Expected Output

When all recipes comply with the style guide:

```
=== Style Guide Compliance Validation ===
Found 20 recipe files to validate

📋 Validating: GitHub/GithubDesktop.fleet.recipe.yaml
   ✅ Filename convention: GithubDesktop.fleet.recipe.yaml (combined format)
   ✅ Vendor folder: GitHub
   ✅ YAML syntax: Valid
   ✅ Required fields: All present (Description, Identifier, Input, Process)
   ✅ Identifier: com.github.fleet.GithubDesktop (combined format)
   ✅ Process stages: 1 (single processor)
   ✅ Processor type: com.github.fleet.FleetImporter/FleetImporter
   ✅ NAME: GitHub Desktop
   ✅ SELF_SERVICE: true
   ✅ AUTOMATIC_INSTALL: false
   ✅ GITOPS_MODE: false (default)
   ✅ FLEET_GITOPS_SOFTWARE_DIR: 'lib/macos/software'
   ✅ FLEET_GITOPS_TEAM_YAML_PATH: 'teams/workstations.yml'
   ✅ CATEGORIES: ['Developer tools'] (required with SELF_SERVICE)
   ✅ Label Targeting: None (valid)
   ✅ Process self_service: '%SELF_SERVICE%'
   ✅ Process automatic_install: '%AUTOMATIC_INSTALL%'
   ✅ Process gitops_software_dir: '%FLEET_GITOPS_SOFTWARE_DIR%'
   ✅ Process gitops_team_yaml_path: '%FLEET_GITOPS_TEAM_YAML_PATH%'
   ✅ Validation complete

[... more recipes ...]

======================================================================
Style Guide Compliance Report
======================================================================

📊 Statistics:
   Total recipes validated: 20
   Combined recipes: 18
   Legacy recipes: 2

🔍 Validation Results:
   Errors: 0
   Warnings: 2

⚠️  Warnings:
   - AgileBits/1Password8.fleet.direct.recipe.yaml: Using legacy recipe format. Consider migrating to combined format (.fleet.recipe.yaml)
   - AgileBits/1Password8.fleet.gitops.recipe.yaml: Using legacy recipe format. Consider migrating to combined format (.fleet.recipe.yaml)

✅ All recipes comply with the style guide!

Validated requirements:
   ✅ YAML syntax is valid
   ✅ Required AutoPkg fields present (Description, Identifier, Input, Process)
   ✅ Filename conventions (.fleet.recipe.yaml or legacy .fleet.direct/gitops.recipe.yaml)
   ✅ Vendor folder structure (no spaces, proper organization)
   ✅ Identifier patterns (com.github.fleet.<Name> for combined, or legacy patterns)
   ✅ Single processor stage (FleetImporter)
   ✅ NAME variable exists in all recipes
   ✅ SELF_SERVICE set to true in all recipes
   ✅ AUTOMATIC_INSTALL set to false in all recipes
   ✅ GITOPS_MODE set to false in combined recipes
   ✅ CATEGORIES required when SELF_SERVICE is true
   ✅ Only one of LABELS_INCLUDE_ANY/LABELS_EXCLUDE_ANY (mutually exclusive)
   ✅ FLEET_GITOPS_SOFTWARE_DIR set to 'lib/macos/software'
   ✅ FLEET_GITOPS_TEAM_YAML_PATH set to 'teams/workstations.yml'
   ✅ Categories use only supported values (when specified)
   ✅ All Process arguments reference Input variables correctly
```

## CI/CD Integration

These tests are automatically run in GitHub Actions on every pull request via the `.github/workflows/validate.yml` workflow.

The style guide compliance test is one of several validation checks that must pass before a PR can be merged:

- Python processor validation
- Environment variable validation
- Recipe structure validation
- Security and best practices check
- Integration test
- **Style guide compliance (includes YAML validation)** ← This test

## Adding New Style Guide Requirements

When adding new requirements to the style guide:

1. Update `CONTRIBUTING.md` with the new requirement
2. Add validation logic to `test_style_guide_compliance.py`
3. Update all existing recipes to comply with the new requirement
4. Run the test locally to verify: `python3 tests/test_style_guide_compliance.py`
5. Commit all changes together

## Troubleshooting

### Test Fails with "Missing SELF_SERVICE"

Ensure the recipe has `SELF_SERVICE: true` in the `Input` section and `self_service: "%SELF_SERVICE%"` in the Process arguments.

### Test Fails with "Missing AUTOMATIC_INSTALL"

Ensure the recipe has `AUTOMATIC_INSTALL: false` in the `Input` section and `automatic_install: "%AUTOMATIC_INSTALL%"` in the Process arguments.

### Test Fails with "Missing GITOPS_MODE"

For combined recipes (`.fleet.recipe.yaml`), ensure the recipe has `GITOPS_MODE: false` in the `Input` section. This variable is required for combined recipes.

### Test Fails with "CATEGORIES is required when SELF_SERVICE is true"

When `SELF_SERVICE: true`, the recipe must include at least one category in the `CATEGORIES` list. For example:
```yaml
Input:
  SELF_SERVICE: true
  CATEGORIES:
    - Developer tools
```

### Test Fails with "Cannot set both LABELS_INCLUDE_ANY and LABELS_EXCLUDE_ANY"

Label targeting with `LABELS_INCLUDE_ANY` and `LABELS_EXCLUDE_ANY` is mutually exclusive. Use only one or neither:
```yaml
Input:
  # Option 1: Include specific labels
  LABELS_INCLUDE_ANY:
    - engineering
    - development
  LABELS_EXCLUDE_ANY: []
  
  # Option 2: Exclude specific labels
  LABELS_INCLUDE_ANY: []
  LABELS_EXCLUDE_ANY:
    - contractors
    
  # Option 3: No label targeting (deploy to all hosts)
  LABELS_INCLUDE_ANY: []
  LABELS_EXCLUDE_ANY: []
```

### Test Fails with GitOps Path Errors

All recipes (both combined and legacy) must include GitOps configuration paths:
- `FLEET_GITOPS_SOFTWARE_DIR: lib/macos/software` in Input section
- `FLEET_GITOPS_TEAM_YAML_PATH: teams/workstations.yml` in Input section
- Both values referenced in Process arguments (for combined recipes)

### Test Fails with Invalid Categories

Ensure categories use only Fleet-supported values:
- `Browsers`
- `Communication`
- `Developer tools`
- `Productivity`

Categories are case-sensitive and must match exactly.

### Warning About Legacy Recipe Format

If you see warnings about legacy recipe format, consider migrating to the combined recipe format:
1. Use the template at `_templates/Template.fleet.recipe.yaml`
2. Follow the migration guide in `CONTRIBUTING.md`
3. The combined format supports both direct and GitOps modes in a single file

## Exit Codes

- `0`: All tests passed
- `1`: One or more tests failed (see error output for details)
