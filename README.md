# Golang Project

## What This Project Does

This project provides a containerized Go development environment using Visual Studio Code's Dev Container feature. It creates an isolated, reproducible workspace with Go 1.25.5 pre-installed, along with essential development tools and VS Code extensions.

The environment is based on Ubuntu 22.04 and includes:
- Go 1.25.5 with standard tooling (gopls, go-outline, gocode, revive)
- Git integration
- Docker-in-Docker capability
- OhMyBash shell configuration
- Preconfigured VS Code extensions for Go, Docker, Python, and productivity tools

## How to Launch

### Prerequisites
- Visual Studio Code with the Dev Containers extension installed
- Docker Desktop running on your machine

### Steps
1. Open this project folder in VS Code
2. When prompted, click "Reopen in Container" (or use Command Palette: `Dev Containers: Reopen in Container`)
3. VS Code will build the Docker image and start the container
4. Once ready, you'll have a full Go development environment

The container automatically:
- Mounts your project files to `/workspaces/<folder-name>`
- Configures Git with your local `.gitconfig` and `.ssh` settings
- Forwards ports 3000, 4000, 6443, 7070, 8080, and 9090 for local development
- Sets Docker socket permissions for Docker-in-Docker operations

## Settings Explanation

### Dockerfile
- **Base Image**: Ubuntu 22.04 with multi-platform support
- **Go Version**: 1.25.5 (configurable via build arg)
- **User Setup**: Creates a non-root user matching your local username with sudo access
- **Go Tools**: Installs gopls (language server), go-outline, gocode, and revive (linter)
- **Shell**: Configured with OhMyBash for enhanced terminal experience

### Docker Compose (compose.yml)
- **Build Context**: Uses local Dockerfile with `GO_VERSION` and `USER_NAME` arguments
- **Network Mode**: `host` mode for direct access to host network
- **Volumes**: Mounts parent directories to `/workspaces` with cached mode for performance
- **Command**: Keeps container running with `sleep infinity`

### Dev Container (devcontainer.json)
- **Workspace Folder**: Maps to `/workspaces/<project-name>` inside container
- **Features**: Adds Git support
- **Extensions**: Auto-installs VS Code extensions for Go, Docker, Python, Markdown, and productivity tools
- **Environment Variables**:
  - `LOCAL_WORKSPACE_FOLDER`: Reference to your host machine path
  - `MODEL_RUNNER_HOST`: Configured for model-runner integration
- **Mounts**:
  - Docker socket for Docker-in-Docker
  - Git configuration for seamless Git operations
  - SSH keys for authentication
- **Remote User**: Uses your local username
- **Post-Create Command**: Configures Git safe directory and Docker socket permissions
