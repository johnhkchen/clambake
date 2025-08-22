# Clambake

**Multi-agent AI orchestration for GitHub repositories.** Clambake coordinates multiple AI coding assistants working on your GitHub Issues simultaneously, preventing conflicts and managing their progress through proper development workflows.

[![Property-Based Tests](https://github.com/johnhkchen/clambake/actions/workflows/property-tests.yml/badge.svg)](https://github.com/johnhkchen/clambake/actions/workflows/property-tests.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-0.1.0-blue.svg)](https://github.com/johnhkchen/clambake)
[![Status](https://img.shields.io/badge/status-Early%20Alpha-red.svg)](https://github.com/johnhkchen/clambake)

## What Clambake Does

- **🤖 Coordinates AI agents** working on different GitHub Issues simultaneously
- **🔀 Prevents conflicts** with automatic branch isolation for each agent
- **⚡ Manages workflow** through a 3-phase cycle: Work → Review → Merge
- **👁️ Uses GitHub natively** - all coordination visible through labels and PRs

**In simple terms:** Scale your development team with AI assistants that work together like human developers.

## Table of Contents
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [Documentation](#documentation)
- [Contributing](#contributing)

## Installation

### Requirements
- **GitHub CLI**: `gh auth login` (for GitHub API access)
- **Git**: Standard git installation
- **Rust**: 1.75+ (for building from source)

### Option 1: Build from Source
```bash
git clone https://github.com/johnhkchen/clambake.git
cd clambake
cargo build --release
```

The executable will be at `./target/release/clambake`.

### Option 2: Pre-built Binaries
Pre-built binaries are planned for future releases.

### Setup Your Repository
After installation, set up the required GitHub labels:
```bash
./target/release/clambake setup-labels
```

> 📖 **Need help?** See the [complete installation guide](docs/README.md#installation) for troubleshooting and advanced configuration.

## Project Status

**Early Alpha** - Not recommended for production use. See [detailed status information](docs/README.md#project-status) for current capabilities and limitations.

## Quick Start

**Already installed?** Jump straight to using Clambake with your GitHub repository.

## Usage Examples

### Basic Agent Workflow

Start your development session by claiming work:

```bash
# Get your next assigned task (primary command)
./target/release/clambake pop
```

This will:
- Assign you the highest priority issue
- Create a dedicated branch (e.g., `agent001/42-fix-bug`)
- Switch you to that branch automatically

### Working on Your Task

Once you have a task, implement your solution:

```bash
# Work in your assigned branch
git add .
git commit -m "Implement feature X"

# Complete your work and create PR
./target/release/clambake land
```

The `land` command:
- Creates a pull request from your branch
- Marks your work ready for review
- Frees you to work on the next task

### System Monitoring

Check what's happening in your repository:

```bash
# View agent status and task queue
./target/release/clambake status
```

Example output:
```
🤖 Agent Status:
  agent001: Working on issue #42 (branch: agent001/42-fix-bug)
  
📋 Task Queue: 3 issues available
  #45: Add user authentication [priority-high]
  #48: Update documentation [priority-medium]  
  #51: Refactor API client [priority-low]
```

### Preview Next Task

See what work is available without claiming it:

```bash
# Preview the next task you would get
./target/release/clambake peek
```

### Complete Daily Workflow Example

Here's a typical development session:

```bash
# 1. Start your day - get first task
./target/release/clambake pop
# ✅ Assigned issue #42: Fix login bug

# 2. Work on the issue (implement your solution)
# ... write code, tests, etc ...
git add .
git commit -m "Fix login validation bug"

# 3. Submit your work
./target/release/clambake land
# ✅ PR created, work submitted for review

# 4. Get next task immediately
./target/release/clambake pop  
# ✅ Assigned issue #45: Add user authentication

# 5. Continue the cycle...
```

### Administrative Commands

**Initialize a new repository:**
```bash
# Set up labels and configuration (run once per repo)
./target/release/clambake init
```

**Reset all agents (admin only):**
```bash
# Clear all agent assignments
./target/release/clambake reset
```

**Bundle multiple PRs for review:**
```bash
# Combine completed work into single review bundle
./target/release/clambake bundle
```

### Getting Help

```bash
# See all available commands
./target/release/clambake --help

# Get help for specific command
./target/release/clambake pop --help
```

## Documentation

Comprehensive documentation is organized for different audiences and use cases:

### 📚 User Documentation
- **[Complete User Guide](docs/README.md)** - Installation, configuration, workflows, and troubleshooting
- **[Command Reference](docs/README.md#commands-commandsmd)** - All CLI commands with examples  
- **[Configuration Guide](docs/README.md#configuration-configurationmd)** - Setup and customization options

### 🏗️ Architecture & Specifications  
- **[System Specification](spec.md)** - Complete system architecture and design principles
- **[Domain Specifications](specs/README.md)** - Detailed technical specifications by domain
- **[API Documentation](https://docs.rs/clambake)** - Auto-generated Rust API docs

### 🤖 Agent Integration
- **[Agent Lifecycle](docs/agent_lifecycle.md)** - How agents coordinate and work together
- **[System Analysis](docs/system_analysis_and_opportunities.md)** - Agent coordination patterns

## Support & Community

Need help? Start with:
- **[Complete Documentation](docs/README.md)** - User guides, troubleshooting, and configuration
- **[GitHub Issues](https://github.com/johnhkchen/clambake/issues)** - Bug reports, feature requests, and questions
- **[System Specification](spec.md)** - Architecture and design principles

## Contributing

We welcome contributions! See the [comprehensive contributing guide](docs/README.md#contributing) for:
- Development setup and guidelines  
- Code quality standards
- Testing approach
- Pull request process

## License

MIT License - see [LICENSE](LICENSE) file for details.

**Copyright © 2025 John Chen**