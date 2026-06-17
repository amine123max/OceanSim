# Contributing to OceanSim

Thank you for your interest in contributing to OceanSim! This guide explains how to participate in development.

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Create a feature branch: `git checkout -b feature/your-feature`
4. Make your changes
5. Submit a Pull Request

## Development Environment

**Requirements:**
- Godot 4.5+
- Python 3.8+
- NumPy

**Setup:**
```bash
# Clone the repository
git clone https://github.com/your-username/OceanSim.git
cd OceanSim/oceansim

# Install Python SDK in editable mode
pip install -e python

# Run the simulator
godot --path .
```

## Code Guidelines

### Godot (GDScript)
- Follow GDScript style guide
- Use descriptive variable names
- Add comments for complex logic
- Keep scripts focused on single responsibilities

### Python SDK
- Follow PEP 8 style guidelines
- Add type hints to public functions
- Write docstrings for public APIs
- Maintain backward compatibility

## Submitting Changes

### Pull Request Process
1. Update documentation if needed
2. Run smoke tests before submitting
3. Ensure all tests pass
4. Write clear commit messages
5. Reference related issues in PR description

### Commit Messages
Use conventional commit format:
```
type(scope): description

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Example: `feat(sensors): add multibeam sonar support`

## Reporting Issues

Use GitHub Issues to report:
- Bugs and errors
- Feature requests
- Documentation issues
- Performance problems

Include:
- Steps to reproduce
- Expected behavior
- Actual behavior
- Environment details (OS, Godot version, Python version)

## Code of Conduct

Please read and follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
