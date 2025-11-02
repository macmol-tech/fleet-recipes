# Contributing to FleetImporter

Thank you for your interest in contributing! Recipe additions and processor enhancements from the community are welcome.

## Ways to Contribute

### Adding New Recipes

We encourage contributions of AutoPkg recipes for additional macOS software. Each software package should have two recipes:

1. **Direct Fleet recipe** (`.fleet.recipe.yaml`) - Uploads directly to Fleet API
2. **GitOps recipe** (`.fleet.gitops.recipe.yaml`) - Uploads to S3 and creates pull requests

See the existing recipes in this repository for examples (Claude, Caffeine, GitHub Desktop, etc.).

#### Recipe Requirements

- Follow the established naming convention: `SoftwareName.fleet.recipe.yaml` and `SoftwareName.fleet.gitops.recipe.yaml`
- Include appropriate categories from Fleet's supported list:
  - Browsers
  - Communication
  - Developer tools
  - Productivity
- Add an icon file (PNG, square, 120-1024px) if available
- Use consistent formatting with existing recipes
- Reference an existing AutoPkg parent recipe that produces a `.pkg` file

#### Recipe Structure

Place recipes in a folder named after the software vendor:

```
VendorName/
├── SoftwareName.fleet.recipe.yaml
├── SoftwareName.fleet.gitops.recipe.yaml
└── SoftwareName.png (optional)
```

### Enhancing the Processor

Improvements to `FleetImporter.py` are welcome, including:

- Bug fixes
- New features that align with Fleet's API capabilities
- Performance improvements
- Better error handling and logging
- Additional configuration options

#### Code Style Requirements

All Python code must pass AutoPkg's code style requirements before submission:

```bash
# Install development dependencies
python3 -m pip install black isort flake8 flake8-bugbear

# Format code
python3 -m black FleetImporter/FleetImporter.py
python3 -m isort FleetImporter/FleetImporter.py

# Validate (all must pass)
python3 -m py_compile FleetImporter/FleetImporter.py
python3 -m black --check --diff FleetImporter/FleetImporter.py
python3 -m isort --check-only --diff FleetImporter/FleetImporter.py
python3 -m flake8 FleetImporter/FleetImporter.py
```

## Submitting Changes

### Before You Submit

1. **Test your changes**: Run recipes locally to ensure they work
2. **Validate YAML**: Ensure all recipe files are valid YAML
3. **Run code checks**: For processor changes, pass all style checks
4. **Update documentation**: Add or update README sections as needed

### Pull Request Process

1. Fork the repository
2. Create a feature branch from `main`
3. Make your changes following the guidelines above
4. Ensure all tests pass in GitHub Actions
5. Submit a pull request with a clear description of your changes

### Pull Request Guidelines

- **Title**: Use a descriptive title (e.g., "Add Firefox recipes" or "Fix error handling in package upload")
- **Description**: Explain what changes you made and why
- **Testing**: Describe how you tested your changes
- **Related Issues**: Link to any related issues or discussions

## Questions or Ideas?

- Open an [issue](https://github.com/kitzy/fleetimporter/issues) for questions or feature requests
- Check existing issues and pull requests before starting work
- Ask for help in the [#autopkg channel](https://macadmins.slack.com/archives/C056155B4) on MacAdmins Slack
- Feel free to ask for help or clarification

## Code of Conduct

Be respectful and constructive in all interactions. This is a community project, and we appreciate your contributions!

## License

By contributing, you agree that your contributions will be licensed under the same license as the project (see [LICENSE](LICENSE)).
