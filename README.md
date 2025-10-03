# Automating Project Versioning and Release Process Using Semantic Versioning and GitHub Actions

Automating versioning and releases is essential for maintaining a smooth and consistent development workflow. By combining Semantic Versioning (SemVer) with GitHub Actions, you can automatically manage version bumps, changelogs, and releases whenever changes are pushed to your repository. This eliminates manual tasks, improves productivity, and ensures a reliable release process.

## 🚀 What This Project Does

This repository demonstrates a complete setup for automated project versioning and release management using:
- **Semantic Versioning (SemVer)**: Automatic version management following MAJOR.MINOR.PATCH format
- **GitHub Actions**: CI/CD automation for release workflows
- **semantic-release**: Fully automatic version management and release publishing
- **Conventional Commits**: Standardized commit message format for automated versioning

## 📋 Table of Contents

- [Understanding the Setup](#understanding-the-setup)
- [What is Semantic Versioning?](#what-is-semantic-versioning)
- [The Role of GitHub Actions](#the-role-of-github-actions)
- [Why Automate Releases?](#why-automate-releases)
- [How Automated Versioning Works](#how-automated-versioning-works)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
- [Commit Message Conventions](#commit-message-conventions)
- [Monitoring Releases](#monitoring-releases)
- [Configuration Files](#configuration-files)

## 🔧 Understanding the Setup

### Key Components

- **Semantic Versioning**: Versioning system following `MAJOR.MINOR.PATCH` format
- **GitHub Actions**: CI/CD tool for automating workflows
- **semantic-release**: Automatic version management and release publishing tool

### Packages Used

- `semantic-release`: Core automation tool
- `@semantic-release/changelog`: Automatic changelog generation
- `@semantic-release/commit-analyzer`: Analyzes commits to determine version bumps
- `@semantic-release/git`: Pushes changes back to repository
- `@semantic-release/github`: Creates GitHub releases automatically

## 📊 What is Semantic Versioning?

Semantic versioning (SemVer) is a versioning scheme that makes it clear whether changes in your project are backward compatible, introduce breaking changes, or simply fix bugs. A typical SemVer version looks like: `MAJOR.MINOR.PATCH` (e.g., v1.4.8)

- **MAJOR**: Incremented when you make incompatible API changes
- **MINOR**: Incremented when you add functionality in a backward-compatible manner
- **PATCH**: Incremented when you make backward-compatible bug fixes

## ⚡ The Role of GitHub Actions

GitHub Actions automates workflows directly in your GitHub repository. These workflows run on various events like code pushes and pull requests. In this setup, GitHub Actions handles the complete release process from building to publishing version updates.

## 🤖 Why Automate Releases?

- **Consistency**: No manual version incrementing or release tagging
- **Efficiency**: Reduced time on repetitive tasks like updating changelogs and version numbers
- **Error-Free**: Eliminates human errors in the release process
- **Reliability**: Repeatable process that can be reused across repositories

## 🔄 How Automated Versioning Works

`semantic-release` automates version management based on your commit messages using Conventional Commits. Here's how it determines version bumps:

### Breaking Changes → Major Version Bump
```bash
feat!: breaking change
# v1.5.2 → v2.0.0
```

### New Features → Minor Version Bump
```bash
feat: add new user login feature
# v2.0.5 → v2.1.0
```

### Bug Fixes → Patch Version Bump
```bash
fix: update documentation
# v4.2.7 → v4.2.8
```

## 📁 Project Structure

```
├── .github/
│   └── workflows/
│       └── release.yml          # GitHub Actions workflow
├── .releaserc                   # semantic-release configuration
├── package.json                 # Project dependencies and metadata
├── package-lock.json           # Lockfile for exact dependency versions
├── README.md                   # This documentation
└── .gitignore                  # Git ignore rules
```

## 🛠 Setup Instructions

### Prerequisites

- Node.js 20.x or later
- Git repository on GitHub
- Basic understanding of Git and npm

### Installation

1. **Clone this repository**:
   ```bash
   git clone https://github.com/ruhulamin63/Automating-Project-Versioning-and-Release-Process.git
   cd Automating-Project-Versioning-and-Release-Process
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Verify installation**:
   ```bash
   npx semantic-release --version
   ```

## 📝 Commit Message Conventions

This project uses [Conventional Commits](https://www.conventionalcommits.org/) to determine version bumps. Follow these formats:

### Types of Commits

- `feat:` - New features (minor version bump)
- `fix:` - Bug fixes (patch version bump)
- `docs:` - Documentation changes
- `style:` - Code style changes
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

### Breaking Changes

For breaking changes, add `!` after the type or include `BREAKING CHANGE:` in the footer:

```bash
feat!: remove deprecated API endpoint

BREAKING CHANGE: This change removes the old authentication method
```

### Examples

```bash
feat: add user authentication system
fix: resolve memory leak in data processing
docs: update API documentation
feat!: redesign user interface (breaking change)
```

## 👀 Monitoring Releases

### GitHub Actions Workflow

1. Go to your repository on GitHub
2. Click the **Actions** tab
3. Select the **Release** workflow
4. Monitor the workflow runs and their status

### Release Process

When you push commits to the `master` branch, the workflow will:

1. **Analyze commits** - Determine the type of release (major/minor/patch)
2. **Update version** - Bump version in `package.json`
3. **Generate changelog** - Create/update `CHANGELOG.md`
4. **Create GitHub release** - Publish release with notes
5. **Push changes** - Commit version and changelog updates

### Viewing Releases

- Go to **Releases** section in your GitHub repository
- Each release will have automatically generated notes
- Tags are created automatically (e.g., `v1.2.3`)

## ⚙️ Configuration Files

### .releaserc

The semantic-release configuration file:

```json
{
  "branches": ["master"],
  "npmPublish": false,
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    ["@semantic-release/github", {}],
    ["@semantic-release/git", {
      "assets": ["package.json", "CHANGELOG.md"],
      "message": "chore(release): ${nextRelease.version} [skip ci]\\n\\n${nextRelease.notes}"
    }]
  ]
}
```

### GitHub Actions Workflow (.github/workflows/release.yml)

```yaml
name: Release

on:
  push:
    branches:
      - master

jobs:
  release:
    permissions:
      contents: write
      issues: write
      pull-requests: write
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "20.x"

      - name: Cache Node.js modules
        uses: actions/cache@v3
        with:
          path: node_modules
          key: ${{ runner.os }}-node-modules-${{ hashFiles('**/package-lock.json') }}

      - name: Install Dependencies
        run: npm ci

      - name: Run Semantic Release
        run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 🎯 Usage

### For Development

1. **Make changes** to your code
2. **Commit with conventional format**:
   ```bash
   git add .
   git commit -m "feat: add new feature description"
   ```
3. **Push to master**:
   ```bash
   git push origin master
   ```
4. **Monitor** the Actions tab for automatic release

### Testing Locally

To test semantic-release locally without publishing:

```bash
npx semantic-release --dry-run
```

This will show you what would happen without actually creating a release.

## 📋 Changelog Example

The system automatically generates changelogs like this:

```markdown
# [1.2.0](https://github.com/ruhulamin63/Automating-Project-Versioning-and-Release-Process/compare/v1.1.0...v1.2.0) (2025-10-03)

### Features

* add automated release workflow ([abc123d](https://github.com/ruhulamin63/Automating-Project-Versioning-and-Release-Process/commit/abc123def456))

### Bug Fixes

* correct semantic-release configuration ([def456g](https://github.com/ruhulamin63/Automating-Project-Versioning-and-Release-Process/commit/def456ghi789))
```

## 🔗 Additional Resources

- [semantic-release Documentation](https://github.com/semantic-release/semantic-release)
- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Semantic Versioning Specification](https://semver.org/)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Commit using conventional commit format
5. Push to your fork
6. Create a Pull Request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**Happy automating!** 🎉

*This setup ensures consistent, error-free versioning and releases, allowing your team to focus on writing code rather than managing releases.*