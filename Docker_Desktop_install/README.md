# Docker Desktop and Kubernetes Installation Guide

This guide provides detailed steps to install Docker Desktop and enable Kubernetes on both Windows and Mac machines.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Windows Installation](#windows-installation)
- [Mac Installation](#mac-installation)
- [Verification](#verification)
- [Troubleshooting](#troubleshooting)

## Prerequisites

### For Windows:
- Windows 10 64-bit: Pro, Enterprise, or Education (Build 19041 or higher)
- Windows 11 64-bit: Home, Pro, Enterprise, or Education
- Virtualization enabled in BIOS/UEFI
- WSL 2 feature enabled (recommended)
- At least 4GB RAM (8GB+ recommended for Kubernetes)

### For Mac:
- macOS must be version 10.15 or newer
- At least 4GB RAM
- Virtualization support (VT-x/AMD-v) enabled
- Apple Silicon (M1/M2) or Intel processor

## Windows Installation

### Step 1: Enable WSL 2 (Windows Subsystem for Linux)
1. Open PowerShell as Administrator
2. Run the following command:
   ```powershell
   wsl --install
   ```
3. Restart your computer when prompted

### Step 2: Install Docker Desktop for Windows
1. Download Docker Desktop from: https://www.docker.com/products/docker-desktop
2. Double-click the installer (Docker Desktop Installer.exe)
3. Follow the installation wizard:
   - Accept the terms
   - Choose "Use WSL 2 instead of Hyper-V" (recommended)
   - Click "Install"
4. Restart your computer when prompted

### Step 3: Enable Kubernetes in Docker Desktop
1. Open Docker Desktop
2. Go to Settings (gear icon in top-right)
3. Navigate to the "Kubernetes" tab
4. Check the box "Enable Kubernetes"
5. Click "Apply & Restart"
6. Wait for Kubernetes to start (this may take several minutes)

### Step 4: Verify WSL 2 Integration
1. Open PowerShell
2. Run:
   ```powershell
   wsl -l -v
   ```
3. You should see Docker Desktop distributions listed

## Mac Installation

### Step 1: Download Docker Desktop
1. Visit: https://www.docker.com/products/docker-desktop
2. Download the appropriate version:
   - Apple Silicon (M1/M2) chip: Download Apple Chip version
   - Intel chip: Download Intel Chip version

### Step 2: Install Docker Desktop
1. Double-click the downloaded .dmg file
2. Drag Docker.app to the Applications folder
3. Open Docker.app from Applications
4. Follow the setup wizard:
   - Grant necessary permissions
   - Accept the terms of service

### Step 3: Enable Kubernetes
1. Click the Docker icon in the menu bar
2. Go to Preferences → Kubernetes
3. Check "Enable Kubernetes"
4. Click "Apply & Restart"
5. Wait for Kubernetes to start (may take 5-10 minutes)

### Step 4: Install Docker CLI Tools (Optional but Recommended)
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Docker CLI tools
brew install docker
brew install kubectl
brew install helm
```

## Verification

### Verify Docker Installation (Both Platforms)
```bash
# Check Docker version
docker --version

# Run test container
docker run hello-world

# Check Docker info
docker info
```

### Verify Kubernetes Installation (Both Platforms)
```bash
# Check Kubernetes version
kubectl version --client

# Check cluster status
kubectl cluster-info

# View nodes
kubectl get nodes

# View all resources
kubectl get all --all-namespaces
```

### Test Kubernetes Deployment
```bash
# Create a test deployment
kubectl create deployment nginx-test --image=nginx

# Expose the deployment
kubectl expose deployment nginx-test --port=80 --type=NodePort

# Check the service
kubectl get services

# Clean up
kubectl delete deployment nginx-test
kubectl delete service nginx-test
```

## Troubleshooting

### Common Windows Issues

**Kubernetes won't start:**
- Ensure virtualization is enabled in BIOS
- Check WSL 2 is properly installed
- Restart Docker Desktop
- Reset Kubernetes cluster in Docker Desktop settings

**Docker daemon not running:**
- Run Docker Desktop as Administrator
- Check Windows Services for Docker services

### Common Mac Issues

**Permission denied errors:**
```bash
# Add your user to docker group
sudo dscl . append /Groups/docker GroupMembership $(whoami)
```

**Kubernetes stuck starting:**
- Reset Kubernetes cluster in Docker Desktop preferences
- Check system resources (CPU/Memory)

### General Troubleshooting Commands

```bash
# Reset Docker to factory defaults (will remove all containers and images)
# Windows: Docker Desktop → Troubleshoot → Reset to factory defaults
# Mac: Docker Desktop → Troubleshoot → Reset to factory defaults

# Check logs
docker logs <container_name>
kubectl get events --all-namespaces

# Remove stuck resources
kubectl delete --all pods --namespace=<namespace>
```

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/)
- [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/)

## Next Steps

After successful installation, you can:
1. Start developing containerized applications
2. Learn Kubernetes basics with minikube tutorials
3. Set up a local development environment
4. Explore Docker Compose for multi-container applications

---

*Note: Always ensure you have the latest version of Docker Desktop for the best experience and security updates.*
