# Elixir Development Container

A VS Code devcontainer configuration for Elixir and Phoenix development.

## Features

- **Elixir 1.18.3** with Erlang/OTP 27.3.4
- **Node.js 20** for Phoenix asset compilation
- **Phoenix framework** pre-installed
- **Development tools**: git, build-essential, inotify-tools, PostgreSQL client
- **VS Code extensions**: Elixir Language Server, Phoenix framework support
- **Non-root user** with sudo access
- **Zsh with Oh My Zsh** for enhanced terminal experience

## Version Configuration

To update the runtime versions, modify the build args in `.devcontainer/devcontainer.json`:

```json
"build": {
  "dockerfile": "Dockerfile",
  "args": {
    "ELIXIR_VERSION": "1.18.3",
    "OTP_VERSION": "27.3.4",
    "NODE_VERSION": "20"
  }
}
```

Available Elixir and OTP version combinations can be found at: https://hub.docker.com/r/hexpm/elixir/tags

## Usage

1. Open the project in VS Code
2. Install the Remote-Containers extension
3. Reopen in Container (Command Palette > "Reopen in Container")
4. The container will automatically run `mix deps.get` if a `mix.exs` file exists

## Release Workflow

### Creating a New Release

1. Update `CHANGELOG.md` with the new version and changes
2. Commit your changes to the main branch
3. Create and push a semantic version tag:
   ```bash
   git tag v0.1.0
   git push origin v0.1.0
   ```
4. GitHub Actions will automatically build and publish the image with these tags:
   - `ghcr.io/jrnk/elixir-devcontainer:0.1.0` (exact version)
   - `ghcr.io/jrnk/elixir-devcontainer:0.1` (major.minor)
   - `ghcr.io/jrnk/elixir-devcontainer:latest` (if from main branch)

### Version Tags

The workflow supports semantic versioning. When you push a tag like `v0.1.0`, it creates:
- Full version tag (0.1.0)
- Major.minor tag (0.1)
- Latest tag (automatically updated from main branch)

### Using the Published Devcontainer Image

Once published, you can reference this image in other projects or devcontainers:

```json
{
  "image": "ghcr.io/jrnk/elixir-devcontainer:latest"
}
```
Or for a specific version:

```json
{
  "image": "ghcr.io/jrnk/elixir-devcontainer:0.1.0"
}
```
