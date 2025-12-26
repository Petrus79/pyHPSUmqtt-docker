# Docker Configuration Guide

This project uses a flexible configuration system that allows you to easily customize:
- The pyHPSU repository source
- The Docker Hub repository where images are published

## Configuration File

The project uses a `docker.config` file for configuration management.

### Setting Up Configuration

1. **Copy the example file:**
   ```bash
   cp docker.config.example docker.config
   ```

2. **Edit `docker.config`** with your preferred values:
   ```bash
   # pyHPSU Repository Configuration
   PYHPSU_REPO=https://github.com/Petrus79/pyHPSUmqtt.git
   
   # Docker Hub Repository Configuration
   DOCKER_HUB_REPO=petrus79/pyhpsu_mqtt
   ```

## GitHub Secrets Configuration

Before the GitHub Actions workflow can push images to Docker Hub, you must configure the following secrets in your repository:

1. **Navigate to GitHub Repository Settings:**
   - Go to your repository on GitHub
   - Click on "Settings" → "Secrets and variables" → "Actions"

2. **Add the following secrets with exact names:**

   - **Secret Name:** `DOCKER_USERNAME`
     - **Value:** Your Docker Hub username
   
   - **Secret Name:** `DOCKER_HUB`
     - **Value:** Your Docker Hub personal access token or password
     - To create a personal access token:
       1. Go to https://hub.docker.com/settings/security
       2. Click "New Access Token"
       3. Give it a descriptive name (e.g., "GitHub Actions")
       4. Copy the token and paste it as the secret value

**Important:** The secret names must exactly match the names used in the workflow file:
- `DOCKER_USERNAME` (case-sensitive)
- `DOCKER_HUB` (case-sensitive)

## GitHub Actions Workflow (Default)

The GitHub Actions workflow is the recommended and default method for building and pushing Docker images.

### How It Works

The GitHub Actions workflow automatically:
1. Reads the `docker.config` file
2. Loads the `PYHPSU_REPO` and `DOCKER_HUB_REPO` values
3. Passes them to the Docker build process
4. Builds and pushes images to your configured Docker Hub repository for all three architectures (amd64, arm32v7, arm64v8)

### Configuration

Simply commit your `docker.config` file to the main branch with your desired values, and the workflow will automatically:
- Trigger on push to main branch
- Use the configuration from `docker.config`
- Build and publish images to Docker Hub with the proper credentials from GitHub Secrets

No additional configuration is needed in the workflow file itself.

## Advanced: Building Locally

For development and testing purposes, you can build Docker images locally with custom configuration.

### Using Custom pyHPSU Repository

You can point to your own fork or different branch:

```bash
# In docker.config
PYHPSU_REPO=https://github.com/your-username/pyHPSUmqtt.git
PYHPSU_REPO=https://github.com/Petrus79/pyHPSUmqtt.git#your-branch
```

### Using Custom Docker Hub Repository

Change your Docker Hub repository:

```bash
# In docker.config
DOCKER_HUB_REPO=your-username/pyhpsu_mqtt
```

### Building Locally

To build Docker images locally with custom configuration:

```bash
# Build amd64 image
docker build \
  --build-arg PYHPSU_REPO=$(grep PYHPSU_REPO docker.config | cut -d'=' -f2) \
  -f Dockerfile-amd64 \
  -t $(grep DOCKER_HUB_REPO docker.config | cut -d'=' -f2):amd64 \
  .

# Build arm32v7 image
docker build \
  --build-arg PYHPSU_REPO=$(grep PYHPSU_REPO docker.config | cut -d'=' -f2) \
  -f Dockerfile-arm32v7 \
  -t $(grep DOCKER_HUB_REPO docker.config | cut -d'=' -f2):arm32v7 \
  .

# Build arm64v8 image
docker build \
  --build-arg PYHPSU_REPO=$(grep PYHPSU_REPO docker.config | cut -d'=' -f2) \
  -f Dockerfile-arm64v8 \
  -t $(grep DOCKER_HUB_REPO docker.config | cut -d'=' -f2):arm64v8 \
  .
```

## GitHub Secrets Configuration


## Important Notes

- The `docker.config` file is used by both GitHub Actions (primary) and local builds
- Make sure to commit your `docker.config` changes to trigger GitHub Actions workflows
- Never commit secrets to the repository; use GitHub Secrets for credentials
- The secrets configured in GitHub are automatically available to the workflow as `${{ secrets.DOCKER_USERNAME }}` and `${{ secrets.DOCKER_HUB }}`
