# Contributing to hyprtoggle

Thank you for your interest in contributing to hyprtoggle! This document provides guidelines for contributing to the project.

## How to Contribute

### Reporting Bugs

If you find a bug, please open an issue with:
- A clear, descriptive title
- Steps to reproduce the issue
- Expected behavior
- Actual behavior
- Your environment (OS, window manager, versions of dependencies)

### Suggesting Enhancements

Enhancement suggestions are welcome! Please open an issue with:
- A clear, descriptive title
- Detailed description of the proposed feature
- Use cases and examples
- Any potential implementation considerations

### Pull Requests

1. Fork the repository
2. Create a new branch for your feature (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test your changes thoroughly
5. Ensure your code passes shellcheck linting
6. Commit your changes with clear, descriptive commit messages
7. Push to your fork (`git push origin feature/amazing-feature`)
8. Open a Pull Request

#### PR Guidelines

- Follow the existing code style
- Add comments for complex logic
- Update documentation if needed
- Ensure all tests pass
- Keep PRs focused on a single feature or fix

## Development Setup

### Requirements

- bash 4.0+
- xdotool
- xprop
- pgrep
- bc

### Testing

Before submitting a PR, test your changes:

```bash
# Check syntax
bash -n hyprtoggle

# Test basic functionality
./hyprtoggle --help
./hyprtoggle --version
```

### Code Style

- Use 4 spaces for indentation (no tabs)
- Quote all variables
- Use `[[ ]]` for conditionals instead of `[ ]`
- Follow shellcheck recommendations
- Add comments for non-obvious logic

## Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Focus on constructive criticism
- Assume good intentions

## Questions?

Feel free to open an issue for questions or clarifications!
