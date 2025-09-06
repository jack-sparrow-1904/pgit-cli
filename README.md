# PGit - Private Git Tracking CLI

[![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/typescript-5.0%2B-blue.svg)](https://www.typescriptlang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A powerful CLI tool that enables developers to version control private files within team repositories using a dual git repository system. Keep your sensitive configuration files, environment variables, and personal notes tracked privately while maintaining complete isolation from your team's shared repository.

## 🚀 What is PGit?

PGit solves the common problem of managing private files in shared repositories. It creates a separate, isolated git repository for your private files while maintaining their original locations through symbolic links. This allows you to:

- **Version control private files** like `.env`, API keys, personal configurations
- **Keep files accessible** at their original paths for applications
- **Maintain complete isolation** from the main team repository
- **Collaborate seamlessly** without exposing private content
- **Track changes** with full git capabilities (commit, log, branch, etc.)

### How It Works

```
Your Project Structure:
├── .git/                    # Main team repository (unchanged)
├── .git-pgit/           # Private git repository (hidden from team)
├── .pgit-storage/       # Actual pgit files storage
│   ├── .env               # Real pgit files stored here
│   └── config.json
├── .env                   # → Symbolic link to .pgit-storage/.env
├── config.json           # → Symbolic link to .pgit-storage/config.json
└── .gitignore            # Automatically excludes pgit system files
```

## 📦 Installation

### Global Installation (Recommended)

```bash
npm install -g pgit-cli
```

### Local Development Installation

```bash
# Clone the repository
git clone https://github.com/jack-sparrow-1904/pgit-cli.git
cd pgit-cli

# Install dependencies
npm install

# Build the project
npm run build

# Link for global use
npm link
```

### Verify Installation

```bash
pgit --version
pgit --help
```

## 🎯 Quick Start

### 1. Initialize Private Tracking

```bash
# Navigate to your project directory
cd your-project

# Initialize pgit git tracking
pgit init
```

This creates:
- `.git-pgit/` - Hidden pgit git repository
- `.pgit-storage/` - Storage for your pgit files
- `.pgit-config.json` - Configuration file
- Updates `.gitignore` to exclude pgit system files

### 2. Add Private Files

```bash
# Add a single file
pgit add .env

# Add multiple files
pgit add config/secrets.json
pgit add .env.local

# Add entire directory
pgit add personal-notes/
```

### 3. Manage Private Repository

```bash
# Check status of both repositories
pgit status

# Stage changes in pgit repository
pgit add-changes --all

# Commit to pgit repository
pgit commit -m "Update environment variables"

# View pgit commit history
pgit log --oneline
```

## 📚 Complete Command Reference

### Initialization Commands

| Command | Description | Example |
|---------|-------------|---------|
| `pgit init` | Initialize dual repository system | `pgit init` |

### File Management Commands

| Command | Description | Options | Example |
|---------|-------------|---------|---------|
| `pgit add <path>` | Add file/directory to pgit tracking | `<path>` (required) | `pgit add .env` |
| `pgit status` | Show status of both repositories | `--verbose`, `-v` | `pgit status -v` |
| `pgit-status` | Show detailed pgit repository status | `--verbose`, `-v` | `pgit-status -v` |

### Git Operations Commands

| Command | Description | Options | Example |
|---------|-------------|---------|---------|
| `pgit commit` | Commit changes to pgit repository | `-m <message>` | `pgit commit -m "update secrets"` |
| `pgit add-changes` | Stage modifications to tracked files | `--all`, `-A` | `pgit add-changes --all` |
| `pgit log` | Show commit history | `--oneline`, `-n <num>` | `pgit log --oneline` |
| `pgit diff` | Show differences in pgit repository | `--cached`, `--name-only` | `pgit diff --cached` |
| `pgit branch` | List or create branches | `<branch-name>` | `pgit branch feature` |
| `pgit checkout` | Switch branches or restore files | `<branch>`, `<file>` | `pgit checkout main` |

### Maintenance Commands

| Command | Description | Options | Example |
|---------|-------------|---------|---------|
| `pgit cleanup` | Fix and repair pgit git tracking | `--force` | `pgit cleanup` |

## 💡 Usage Examples

### Managing Environment Files

```bash
# Initialize in your project
pgit init

# Add environment files
pgit add .env
pgit add .env.local
pgit add .env.development

# Your app can still read from .env (symbolic link)
# But the actual file is pgitly tracked

# Make changes and commit
pgit add-changes --all
pgit commit -m "Update API endpoints"
```

### Working with Configuration Files

```bash
# Add configuration files
pgit add config/database.json
pgit add config/api-keys.json

# Create a feature branch for testing
pgit branch testing-config
pgit checkout testing-config

# Make changes and commit
pgit add-changes --all
pgit commit -m "Test new database config"

# Switch back to main
pgit checkout main
```

### Checking Repository Status

```bash
# Quick status check
pgit status

# Output:
# 📊 Private Git Tracking Status
# ✓ System is healthy
# 📋 Main Repository - Branch: main, Status: Clean
# 🔒 Private Repository - Branch: main, Status: Has changes

# Detailed pgit repository status
pgit-status --verbose

# View commit history
pgit log --oneline
```

## 🔧 Advanced Features

### Branch Management

Create separate branches for different environments or configurations:

```bash
# Create development branch
pgit branch development
pgit checkout development

# Make dev-specific changes
echo "DEV_API_URL=http://localhost:3000" >> .env
pgit add-changes --all
pgit commit -m "Add development API URL"

# Switch between configurations
pgit checkout main      # Production config
pgit checkout development # Development config
```

### Collaborative Workflows

While pgit files remain pgit, you can share configurations with trusted team members:

```bash
# Export pgit repository (to share with trusted colleagues)
cd .pgit-storage
git remote add origin git@github.com:your-org/project-pgit.git
git push -u origin main

# Another team member can clone and set up
git clone git@github.com:your-org/project-pgit.git .pgit-storage
pgit cleanup  # Repairs symbolic links
```

### Backup and Recovery

```bash
# Your pgit repository is a full git repository
cd .pgit-storage
git remote add backup git@github.com:your-org/project-backup.git
git push backup main

# To restore after cleanup or system changes
pgit cleanup --force
```

## 🛠️ How to Contribute

We welcome contributions to the Private Git Tracking CLI! Here's how you can help:

### Development Setup

1. **Fork and Clone**
   ```bash
   git clone https://github.com/your-username/pgit-cli.git
   cd pgit-cli
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Development Workflow**
   ```bash
   # Run in development mode
   npm run dev -- init

   # Build the project
   npm run build

   # Run tests
   npm test

   # Run tests in watch mode
   npm run test:watch

   # Lint code
   npm run lint

   # Format code
   npm run format
   ```

### Project Structure

```
pgit-git-cli/
├── src/
│   ├── commands/              # Command implementations
│   │   ├── init.command.ts
│   │   ├── add.command.ts
│   │   ├── status.command.ts
│   │   └── ...
│   ├── core/                  # Core business logic
│   │   ├── config.manager.ts
│   │   ├── git.service.ts
│   │   ├── symlink.service.ts
│   │   └── filesystem.service.ts
│   ├── types/                 # TypeScript type definitions
│   ├── utils/                 # Utility functions
│   ├── errors/                # Custom error classes
│   └── cli.ts                 # CLI entry point
├── tests/                     # Test files
├── docs/                      # Documentation
└── dist/                      # Built output
```

### Contribution Guidelines

#### 1. Code Standards

- **TypeScript**: Use strict mode with full type safety
- **ESLint**: Follow the established linting rules (`npm run lint`)
- **Prettier**: Use consistent formatting (`npm run format`)
- **Testing**: Maintain >90% test coverage (`npm run test`)

#### 2. Commit Standards

```bash
# Use conventional commit format
git commit -m "feat: add symbolic link validation"
git commit -m "fix: handle Windows junction points correctly"
git commit -m "docs: update installation instructions"
git commit -m "test: add cross-platform symbolic link tests"
```

#### 3. Pull Request Process

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes**
   - Write code following project standards
   - Add tests for new functionality
   - Update documentation as needed

3. **Test Your Changes**
   ```bash
   npm run test
   npm run lint
   npm run build
   
   # Test CLI functionality
   npm link
   cd /tmp/test-project
   git init
   pgit init
   echo "test" > .env
   pgit add .env
   ```

4. **Submit Pull Request**
   - Clear description of changes
   - Reference any related issues
   - Include testing instructions

#### 4. Areas for Contribution

**🐛 Bug Fixes**
- Cross-platform compatibility issues
- Symbolic link handling edge cases
- Git operation error handling
- Performance optimizations

**✨ New Features**
- Additional git operations (merge, rebase, stash)
- Configuration templates
- Integration with popular development tools
- Backup and sync features

**📚 Documentation**
- Tutorial guides
- Video examples
- API documentation
- Troubleshooting guides

**🧪 Testing**
- Cross-platform test coverage
- Performance benchmarks
- Integration test scenarios
- Error condition testing

#### 5. Development Tips

**Running Local Tests**
```bash
# Test specific functionality
npm test -- --testNamePattern="ConfigManager"

# Test with coverage
npm run test:coverage

# Test in different environments
docker run -v $(pwd):/app -w /app node:18 npm test
```

**Debugging**
```bash
# Enable verbose output
npm run dev -- status --verbose

# Debug with Node.js debugger
npm run dev:debug -- init
```

**Testing Commands Locally**
```bash
# Build and link for testing
npm run build && npm link

# Test in a temporary directory
mkdir /tmp/test-project
cd /tmp/test-project
git init
pgit init
```

### Code of Conduct

We are committed to providing a welcoming and inclusive environment for all contributors. Please:

- Be respectful and constructive in discussions
- Follow the established coding standards
- Write clear commit messages and documentation
- Test your changes thoroughly
- Help others learn and grow

### Getting Help

**📞 Questions?**
- Open a [Discussion](https://github.com/jack-sparrow-1904/pgit-cli/discussions)
- Join our [Discord community](https://discord.gg/your-server)
- Check the [Wiki](https://github.com/jack-sparrow-1904/pgit-cli/wiki)

**🐛 Found a Bug?**
- Open an [Issue](https://github.com/jack-sparrow-1904/pgit-cli/issues)
- Include reproduction steps
- Specify your operating system and Node.js version

**💡 Feature Ideas?**
- Open a [Feature Request](https://github.com/jack-sparrow-1904/pgit-cli/issues/new?template=feature_request.md)
- Describe the use case and expected behavior
- Discuss implementation approach

## 🔒 Security & Privacy

- **No data collection**: The CLI operates entirely locally
- **No network requests**: All operations are performed on your local machine
- **Private files stay pgit**: Only you control access to your pgit repository
- **Symbolic links are secure**: Files remain in their expected locations for applications

## 📋 System Requirements

- **Node.js**: 18.0.0 or higher
- **Operating System**: macOS, Linux, or Windows
- **Git**: Must be installed and available in PATH
- **File System**: Must support symbolic links (most modern systems do)

## 🔍 Troubleshooting

### Common Issues

**Q: Symbolic links not working on Windows?**
```bash
# Enable Developer Mode or run as Administrator
# Then run cleanup to recreate links
pgit cleanup --force
```

**Q: Files not showing up in pgit repository?**
```bash
# Check status and stage changes
pgit status
pgit add-changes --all
pgit commit -m "Add missing files"
```

**Q: Main repository shows pgit files?**
```bash
# Ensure .gitignore is updated
pgit cleanup
git status  # Should not show pgit files
```

**Q: Permission denied errors?**
```bash
# Check file permissions
ls -la .pgit-storage/
# Fix permissions if needed
chmod 644 .pgit-storage/*
```

### Getting Support

1. **Check the logs**: Enable verbose output with `--verbose` flag
2. **Run cleanup**: `pgit cleanup --force` fixes most issues
3. **Check system requirements**: Ensure Node.js 18+ and git are installed
4. **Open an issue**: Include error messages and system information

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with [Commander.js](https://github.com/tj/commander.js/) for CLI framework
- Uses [simple-git](https://github.com/steveukx/git-js) for git operations
- Styled with [Chalk](https://github.com/chalk/chalk) for colored output
- File operations powered by [fs-extra](https://github.com/jprichardson/node-fs-extra)
- Type safety with [Zod](https://github.com/colinhacks/zod) schemas

---

**Made with ❤️ by developers, for developers who value privacy and clean git workflows.**