# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **PGit** - a CLI tool for private file tracking using dual git repositories. The tool allows developers to version control private files (like `.env`, API keys, personal configurations) within team repositories while maintaining complete isolation from the main team repository using symbolic links.

## Development Commands

### Building and Testing
- `npm run build` - Compile TypeScript to dist/ directory
- `npm run dev -- <command>` - Run CLI commands in development (e.g., `npm run dev -- init`)
- `npm test` - Run Jest test suite (requires 90% coverage)
- `npm run test:watch` - Run tests in watch mode

### Code Quality
- `npm run lint` - Run ESLint on TypeScript files
- `npm run lint:fix` - Auto-fix ESLint issues
- `npm run format` - Format code with Prettier

### Other Commands
- `npm run clean` - Remove dist/ directory
- `npm run prepublishOnly` - Build and test before publishing

## Architecture

### Core Structure
```
src/
├── cli.ts                     # Main CLI entry point using Commander.js
├── commands/                  # Command implementations
│   ├── init.command.ts       # Initialize private git tracking
│   ├── add.command.ts        # Add files to private tracking
│   ├── status.command.ts     # Show repository status
│   ├── commit.command.ts     # Commit to private repository
│   ├── gitops.command.ts     # Git operations (log, diff, branch, checkout)
│   └── cleanup.command.ts    # Repair and cleanup operations
├── core/                     # Core business logic services
│   ├── config.manager.ts     # Configuration management
│   ├── git.service.ts        # Git operations wrapper
│   ├── filesystem.service.ts # File system operations
│   └── symlink.service.ts    # Symbolic link management
├── types/                    # TypeScript types and Zod schemas
│   ├── config.types.ts       # Configuration type definitions
│   └── config.schema.ts      # Zod validation schemas
├── utils/                    # Utility functions
├── errors/                   # Custom error classes with enhanced handling
└── __tests__/               # Jest test files
```

### Key Concepts
- **Dual Repository System**: Main team repository (`.git/`) + private repository (`.git-pgit/`)
- **Symbolic Links**: Files stored in `.pgit-storage/` with symlinks at original locations
- **Configuration**: `.private-config.json` manages tracking settings
- **Command Pattern**: Each CLI command is implemented as a separate class with `execute()` method

### Dependencies
- **Commander.js**: CLI framework for command parsing
- **simple-git**: Git operations wrapper
- **Zod**: Runtime type validation and schema parsing
- **Chalk**: Colored console output
- **fs-extra**: Enhanced file system operations

## Code Conventions

### TypeScript Configuration
- Strict mode enabled with comprehensive type checking
- Target: ES2020, Module: CommonJS
- Compilation output: `dist/` directory
- Declaration files generated for type safety

### Testing Requirements
- Jest with ts-jest preset
- Minimum 90% coverage required (branches, functions, lines, statements)
- Test files: `**/__tests__/**/*.ts` and `**/*.test.ts`
- Coverage reports: text, lcov, html formats

### Linting and Formatting
- ESLint with TypeScript recommended rules
- Prettier for consistent code formatting
- Key rules: no-console allowed, prefer-const, explicit return types, no any
- Single quotes, trailing commas in multiline

## Development Notes

### Entry Points
- CLI binary: `pgit` (points to `dist/cli.js`)
- Development: `npm run dev -- <command>`
- Main module: `dist/index.js`

### Error Handling
- Custom error classes extending `BaseError` with error codes
- Enhanced error handler with recovery suggestions
- Recoverable vs non-recoverable error classification

### File System Operations
- Cross-platform symbolic link support
- Platform detection for OS-specific operations
- Atomic file operations with rollback capability

### Git Integration
- Operates on both main repository and private `.git-pgit/` repository
- Maintains separate commit histories
- Automatic `.gitignore` management for private files