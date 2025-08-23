# My Little Soda

**Autonomous AI agent orchestration for GitHub repositories.** 

My Little Soda enables a single autonomous AI coding assistant to work on your GitHub Issues continuously while you focus elsewhere. It provides unattended operation and multiplicative productivity through the one-agent-per-repo architecture.

[![Property-Based Tests](https://github.com/johnhkchen/my-little-soda/actions/workflows/property-tests.yml/badge.svg)](https://github.com/johnhkchen/my-little-soda/actions/workflows/property-tests.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-0.1.0-blue.svg)](https://github.com/johnhkchen/my-little-soda)
[![Status](https://img.shields.io/badge/status-Early%20Alpha-red.svg)](https://github.com/johnhkchen/my-little-soda)

## What My Little Soda Does

- **🤖 Autonomous operation** - Single AI agent works continuously on GitHub Issues while you focus elsewhere
- **🔄 Multiplicative productivity** - 8 hours human work + 3 autonomous repos = 32 repo-hours of progress
- **⚡ Seamless workflow** through a 3-phase cycle: Work → Review → Merge
- **👁️ GitHub native** - All coordination visible through labels and PRs

**In simple terms:** Scale your productivity with an autonomous AI assistant that works unattended on your repository.

## Table of Contents
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [Documentation](#documentation)
- [Contributing](#contributing)

## Installation

### Prerequisites

Before installing My Little Soda, ensure you have the following:

#### Required
- **GitHub CLI**: `gh auth login` (for GitHub API access)
  - Install: https://cli.github.com/
  - Authenticate: `gh auth login` (required for repository operations)
  - Verify: `gh auth status`
- **Git**: Standard git installation
- **Rust**: 1.75+ (for building from source)
- **GitHub Personal Access Token**: Required for API operations
  - Create at: https://github.com/settings/tokens
  - Required scopes: `repo`, `read:org` (for private repos)
  - Can be set via `GITHUB_TOKEN` or `MY_LITTLE_SODA_GITHUB_TOKEN` environment variable

#### Repository Permissions
- **Write access** to the target repository (for creating branches, PRs, and labels)
- **Issues permission** (to read, create, and modify issues)
- **Pull requests permission** (to create and manage PRs)

#### Optional Dependencies
- **Database** (SQLite): For persistent state storage and metrics
  - Auto-created at `.my-little-soda/my-little-soda.db` if enabled
  - Enable in `my-little-soda.toml` or via `MY_LITTLE_SODA_DATABASE_URL`
- **OpenTelemetry Endpoint**: For distributed tracing and observability
  - Defaults to stdout export if not configured
  - Set via `CLAMBAKE_OBSERVABILITY_OTLP_ENDPOINT`

> **Note**: My Little Soda is a coordination tool for GitHub repositories. It does not require API keys for AI services (OpenAI, Anthropic, etc.) as it manages workflows for an external autonomous AI agent that handles its own authentication.

### Platform Support
- **Linux** (x86_64, aarch64)
- **macOS** (Intel and Apple Silicon)  
- **Windows** (Windows 10/11)

> **Windows Note:** Use `.\target\release\my-little-soda.exe` instead of `./target/release/my-little-soda`

### Option 1: Build from Source

```bash
git clone https://github.com/johnhkchen/my-little-soda.git
cd my-little-soda
cargo build --release
```

Executable location: `./target/release/my-little-soda` (Windows: `.\target\release\my-little-soda.exe`)

### Option 2: Pre-built Binaries
Pre-built binaries are planned for future releases.

### Configuration

Clambake supports multiple configuration methods in order of precedence:

#### Option 1: Environment Variables (Recommended for CI/CD)
```bash
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"
export CLAMBAKE_GITHUB_OWNER="your-username"
export CLAMBAKE_GITHUB_REPO="your-repo"
```

#### Option 2: Configuration File (Recommended for local development)
Copy the example configuration and customize:
```bash
cp my-little-soda.example.toml my-little-soda.toml
# Edit my-little-soda.toml with your repository details
```

#### Option 3: .env File
Create a `.env` file in your project root:
```bash
GITHUB_TOKEN=ghp_xxxxxxxxxxxxx
CLAMBAKE_GITHUB_OWNER=your-username
CLAMBAKE_GITHUB_REPO=your-repo
```

### Setup Your Repository

#### Option 1: Automated Setup (Coming Soon)
The `my-little-soda init` command will automate repository setup in a future release:

```bash
# Future: One-command setup (WIP)
./target/release/my-little-soda init
```

**What this will do:**
- ✅ Validate GitHub authentication and permissions
- 🏷️  Create required routing labels (`route:ready`, `route:priority-high`, etc.)
- ⚙️  Generate `my-little-soda.toml` configuration 
- 🤖 Initialize autonomous agent configuration
- 📁 Create `.my-little-soda/` directory structure
- ✅ Verify setup and test connectivity

#### Option 2: Manual Setup (Current Required Process)
Until `my-little-soda init` is implemented, set up your repository manually:

**1. Create Required GitHub Labels:**
```bash
# Core routing labels
gh label create "route:ready" --color "0052cc" --description "Available for agent assignment"
gh label create "route:ready_to_merge" --color "5319e7" --description "Completed work ready for merge"
gh label create "route:unblocker" --color "d73a4a" --description "Critical system issues"
gh label create "route:review" --color "fbca04" --description "Under review"
gh label create "route:human-only" --color "7057ff" --description "Requires human attention"

# Priority labels  
gh label create "route:priority-low" --color "c2e0c6" --description "Priority: 1"
gh label create "route:priority-medium" --color "f9d71c" --description "Priority: 2"
gh label create "route:priority-high" --color "ff6b6b" --description "Priority: 3"  
gh label create "route:priority-very-high" --color "d73a4a" --description "Priority: 4"
```

**2. Verify Configuration:**
```bash
# Test that my-little-soda can connect to your repository
./target/release/my-little-soda status
```

**3. Start Using My Little Soda:**
```bash
# Label some issues as ready for the agent
gh issue edit <issue-number> --add-label "route:ready"

# Begin agent workflow
./target/release/my-little-soda pop
```

> 📖 **Need help?** See the [complete installation guide](docs/README.md#installation) for troubleshooting and advanced configuration.

## Project Status

**Early Alpha** - Not recommended for production use. See [detailed status information](docs/README.md#project-status) for current capabilities and limitations.

## Quick Start

**Already installed?** Here's the essential workflow:

1. **Get a task:** `./target/release/my-little-soda pop`
2. **Work on it:** Make your changes and commit
3. **Submit work:** `./target/release/my-little-soda bottle`
4. **Repeat:** System automatically assigns next task

See [Usage Examples](#usage-examples) for detailed commands.

## Usage Examples

### Basic Agent Workflow

Start your development session by claiming work:

```bash
# Get your next assigned task (primary command)
./target/release/my-little-soda pop
```

**What this does:**
- Assigns you the highest priority issue
- Creates a dedicated branch (e.g., `agent001/42-fix-bug`)
- Switches you to that branch automatically

### Working on Your Task

Once you have a task, implement your solution:

```bash
# Work in your assigned branch
git add .
git commit -m "Implement feature X"

# Complete your work and create PR
./target/release/my-little-soda bottle
```

**What `land` does:**
- Creates a pull request from your branch
- Marks your work ready for review
- Frees you to work on the next task

### System Monitoring

Check what's happening in your repository:

```bash
# View agent status and task queue
./target/release/my-little-soda status
```

Example output:
```
🤖 Agent Status:
  agent001: Working on issue #42 (branch: agent001/42-fix-bug)
  Uptime: 4h 23m | Issues processed: 7 | Average time: 22m
  
📋 Task Queue: 3 issues available
  #45: Add user authentication [priority-high]
  #48: Update documentation [priority-medium]  
  #51: Refactor API client [priority-low]
```

### Preview Next Task

See what work is available without claiming it:

```bash
# Preview the next task you would get
./target/release/my-little-soda peek
```

### Complete Daily Workflow Example

Here's a typical development session:

```bash
# 1. Start your day - get first task
./target/release/my-little-soda pop
# ✅ Assigned issue #42: Fix login bug

# 2. Work on the issue (implement your solution)
# ... write code, tests, etc ...
git add .
git commit -m "Fix login validation bug"

# 3. Submit your work
./target/release/my-little-soda bottle
# ✅ PR created, work submitted for review

# 4. Get next task immediately
./target/release/my-little-soda pop  
# ✅ Assigned issue #45: Add user authentication

# 5. Continue the cycle...
```

### Administrative Commands

```bash
# Initialize a new repository (run once per repo)
./target/release/my-little-soda init

# Reset agent state (admin only)
./target/release/my-little-soda reset

# Bundle multiple PRs for review
./target/release/my-little-soda bundle
```

### Getting Help

```bash
# See all available commands
./target/release/my-little-soda --help

# Get help for specific command
./target/release/my-little-soda pop --help
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
- **API Documentation** - Auto-generated Rust API docs (available after crate publication)

### 🤖 Agent Integration
- **[Agent Lifecycle](docs/agent_lifecycle.md)** - How autonomous agent operates and processes issues
- **[System Analysis](docs/system_analysis_and_opportunities.md)** - Autonomous agent operation patterns

## Support & Community

**Need help? Start with:**
- **[Complete Documentation](docs/README.md)** - User guides, troubleshooting, and configuration
- **[GitHub Issues](https://github.com/johnhkchen/my-little-soda/issues)** - Bug reports, feature requests, and questions
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