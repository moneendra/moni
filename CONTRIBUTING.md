# Contributing to Smart Parking System

Thank you for your interest in contributing to the Smart Parking System! This document provides guidelines and instructions for contributing.

## Code of Conduct

Be respectful, inclusive, and professional in all interactions.

## Getting Started

### Prerequisites
- Git installed
- A GitHub account
- Basic knowledge of HTML, CSS, JavaScript

### Setup
1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/moni.git
   cd moni
   ```
3. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. Make your changes
5. Test thoroughly in multiple browsers

## Types of Contributions

### 🐛 Bug Reports
- Use clear, descriptive titles
- Describe the exact steps to reproduce
- Include browser and OS information
- Share screenshots if applicable

### ✨ Feature Requests
- Describe the desired functionality
- Explain the use case
- Provide mockups if helpful

### 📝 Documentation
- Improve README clarity
- Add code examples
- Fix typos and grammar

### 💻 Code Changes
- Fix bugs
- Add features
- Improve performance
- Refactor code

## Development Workflow

### Before Starting
1. Check for existing issues/PRs
2. Discuss major changes in an issue first

### Making Changes
1. Follow the existing code style
2. Keep commits atomic and logical
3. Use descriptive commit messages
4. Test in Chrome, Firefox, Safari, and Edge

### Commit Messages
```
Type: Brief description

Detailed explanation if needed
- List bullet points for complex changes
- Reference issue numbers: Closes #123
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`

### Example
```
feat: Add QR code generation for bookings

- Generate QR codes with booking details
- Display in booking confirmation
- Allows offline verification
Closes #42
```

## Pull Request Process

1. Update README.md with new features
2. Test the complete workflow
3. Ensure code is clean and commented
4. Link related issues
5. Provide before/after screenshots for UI changes
6. Wait for maintainer review

### PR Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Other

## Testing
- [ ] Tested in Chrome
- [ ] Tested in Firefox
- [ ] Tested in Safari
- [ ] Tested on mobile
- [ ] Tested dark mode

## Screenshots (if applicable)
Before/After images

## Related Issues
Closes #123
```

## Coding Standards

### HTML
- Semantic HTML5 elements
- Proper heading hierarchy
- Descriptive IDs and classes
- Comments for complex sections

### CSS
- Use CSS custom properties for colors
- Mobile-first responsive design
- Organize by component
- Use meaningful variable names

### JavaScript
- ES6+ syntax
- Descriptive variable/function names
- Comments for complex logic
- No console errors/warnings

### Style Guide
```javascript
// ✅ Good
const currentUser = getUserName();
function calculateOccupancy() {
  // Clear logic
}

// ❌ Avoid
const cu = getUserName();
function calc() {
  // Unclear
}
```

## Reporting Issues

### Bug Report Template
```markdown
## Description
What is the bug?

## Steps to Reproduce
1. Step one
2. Step two
3. ...

## Expected Behavior
What should happen?

## Actual Behavior
What actually happens?

## Environment
- Browser: [e.g., Chrome 100]
- OS: [e.g., Windows 10]
- Screen size: [e.g., 1920x1080]

## Screenshots
[If applicable]
```

### Feature Request Template
```markdown
## Description
What feature would you like?

## Use Case
Why do you need this?

## Proposed Solution
How should it work?

## Alternatives
Any alternatives considered?
```

## Review Process

1. Maintainer reviews your PR
2. Provide feedback or request changes
3. Update your branch with requested changes
4. Request re-review
5. PR is merged once approved

## Community

- Ask questions in Issues
- Start Discussions for ideas
- Be patient and respectful
- Help review others' PRs

## Recognition

Contributors are recognized in:
- README.md contributors section
- GitHub contributors page
- Release notes for significant contributions

## Questions?

- Check existing documentation
- Search closed issues/discussions
- Open a new discussion
- Comment on relevant issue

## License

By contributing, you agree your work will be licensed under the MIT License.

---

**Thank you for contributing to make Smart Parking System better!** 🚗✨
