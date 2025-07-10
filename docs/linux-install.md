# Linux Installation Guide

This guide will help you set up the ROS2 PX4 development environment on Linux.

## Prerequisites

- Ubuntu 20.04 LTS or later (recommended: Ubuntu 22.04 LTS)
- At least 8GB RAM (16GB recommended)
- 20GB free disk space

## Step 1: Install Docker

### Ubuntu/Debian

1. Update package index:

   ```bash
   sudo apt update
   ```

2. Install dependencies:

   ```bash
   sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
   ```

3. Add Docker's official GPG key:

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. Add Docker repository:

   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

5. Install Docker:

   ```bash
   sudo apt update
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

6. Add your user to the docker group:

   ```bash
   sudo usermod -aG docker $USER
   ```

7. Log out and log back in, then test Docker:

   ```bash
   docker run hello-world
   ```

## Step 2: Install Visual Studio Code

### Using Snap (Recommended)

```bash
sudo snap install --classic code
```

### Using APT

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code
```

## Step 3: Install VS Code Extensions

1. Launch VS Code:

   ```bash
   code
   ```

2. Install the Dev Containers extension:
   - Press `Ctrl+Shift+X` to open Extensions
   - Search for "Dev Containers" and install it
   - Also install "Docker" extension for additional Docker support

## Step 4: Set Up X11 Forwarding

For GUI applications to work in the container, ensure X11 is properly configured:

1. Install X11 utilities:

   ```bash
   sudo apt install x11-xserver-utils
   ```

2. Allow local connections (run this each time you start a new session, or add to `.bashrc`):

   ```bash
   xhost +local:docker
   ```

3. Test X11 forwarding:

   ```bash
   xclock  # Should display a clock window
   ```

## Step 5: Clone and Run the Project

1. Clone the repository:

   ```bash
   git clone <repository-url>
   cd ros2_px4_startup
   ```

2. Open in VS Code:

   ```bash
   code .
   ```

3. When prompted, click "Reopen in Container" or use `Ctrl+Shift+P` → "Dev Containers: Reopen in Container"

## Troubleshooting

### Docker Permission Issues

**Problem**: "permission denied while trying to connect to the Docker daemon socket"

```bash
# Check if your user is in docker group:
groups $USER
# If docker is not listed, add user to group:
sudo usermod -aG docker $USER
# Log out and log back in
```

### X11 Display Issues

**Problem**: "cannot open display" errors for GUI applications

```bash
# Check DISPLAY variable:
echo $DISPLAY
# Should show something like :0 or :1

# Allow X11 forwarding:
xhost +local:docker

# If using Wayland, you might need to switch to X11:
# Log out, click gear icon on login screen, select "Ubuntu on Xorg"
```

### Container Build Issues

**Problem**: Container fails to build due to network issues

```bash
# Try building with different DNS:
docker build --dns 8.8.8.8 .
```

**Problem**: "no space left on device"

```bash
# Clean up Docker:
docker system prune -a
docker volume prune
```

## Hardware Integration

### USB Devices (Flight Controllers)

1. Add your user to the dialout group:

   ```bash
   sudo usermod -aG dialout $USER
   ```

2. For specific USB devices, you might need to add udev rules:

   ```bash
   # Example for PX4 flight controllers:
   echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="26ac", ATTRS{idProduct}=="0011", MODE="0666"' | sudo tee /etc/udev/rules.d/99-px4.rules
   sudo udevadm control --reload-rules
   ```

## Next Steps

Once everything is set up, return to the main [README](../README.md) to continue with the Quick Start guide and usage instructions.
