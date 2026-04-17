# GitHub Container Registry (GHCR) Usage Notes

This document provides a quick reference for authenticating, tagging, pushing, and pulling Docker images using the GitHub Container Registry (GHCR).

## 1. Authentication (Login)
To interact with GHCR, you must first authenticate using a Personal Access Token (PAT) with at least the `read:packages`, `write:packages`, and `delete:packages` scopes.

```bash
# Set your GitHub Personal Access Token as an environment variable
export CR_PAT="YOUR_PERSONAL_ACCESS_TOKEN"

# Log in to the GitHub Container Registry
# Replace YOUR_GITHUB_USERNAME with your GitHub username (always use lowercase)
echo $CR_PAT | docker login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin
```

Alternatively, you can use the interactive login:
```bash
docker login ghcr.io
# Provide your GitHub username when prompted
# Provide your PAT when prompted for the password
```

## 2. Tagging an Image
Before pushing an image to GHCR, it must be tagged with the GHCR registry URL, your GitHub username (or organization), and the repository/image name. **GitHub user/org and image names must be in lowercase.**

**Format:**
```bash
docker tag local-image:tag ghcr.io/your_github_username/image-name:tag
```

**Example:**
```bash
docker tag my-node-app:latest ghcr.io/pawan/my-node-app:v1.0
```

## 3. Pushing the Image
Once authenticated and tagged, you can push the image to your registry.

**Format:**
```bash
docker push ghcr.io/your_github_username/image-name:tag
```

**Example:**
```bash
docker push ghcr.io/pawan/my-node-app:v1.0
```

> **Note:** By default, pushed images are often bound to your personal account or repository, and their default visibility is "Private". You can change this to "Public" via the GitHub UI under your Profile > Packages or Repository > Packages.

## 4. Pulling the Image
To pull the image from GHCR on another machine or environment (ensure you are logged in if the package visibility is set to Private).

**Format:**
```bash
docker pull ghcr.io/your_github_username/image-name:tag
```

**Example:**
```bash
docker pull ghcr.io/pawan/my-node-app:v1.0
```

## Summary Checklist
1. **Generate PAT** with `write:packages`, `read:packages`, `delete:packages`.
2. **Login:** `echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin`
3. **Tag:** `docker tag my-image:latest ghcr.io/USERNAME/my-image:version`
4. **Push:** `docker push ghcr.io/USERNAME/my-image:version`
5. **Pull:** `docker pull ghcr.io/USERNAME/my-image:version`
