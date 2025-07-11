# Windows Installation Guide

This guide will help you set up the ROS2 PX4 development environment on Windows using WSL2 and Docker Desktop.

## Prerequisites

- Windows 10 version 2004 or higher, or Windows 11
- At least 8GB RAM (16GB recommended)
- 20GB free disk space

## Step 1: Install WSL2

1. Open PowerShell as Administrator and run:

   ```powershell
   wsl --install
   ```

2. Restart your computer when prompted.

3. After restart, set up your Ubuntu username and password.

4. Update WSL2 to the latest version:

   ```powershell
   wsl --update
   ```

## Step 2: Install Visual Studio Code

1. Download VS Code from [https://code.visualstudio.com/](https://code.visualstudio.com/)

2. Install with default settings.

3. Install the following extensions:
   - **Remote - WSL**
   - **Dev Containers**
   - **Docker**

## Step 3: Install Docker Desktop

### Option A: Traditional Installation

1. Download Docker Desktop from [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

2. Run the installer with default settings.

3. During installation, ensure "Use WSL 2 instead of Hyper-V" is selected.

4. Restart your computer after installation.

5. Launch Docker Desktop and complete the initial setup.

6. In Docker Desktop settings:
   - Go to **General** → Enable "Use the WSL 2 based engine"
   - Go to **Resources** → **WSL Integration** → Enable integration with your Ubuntu distribution

### Option B: Install via VS Code Dev Containers Extension (Easier)

1. Open VS Code and install the **Dev Containers** extension from the Extensions marketplace (if not already installed from Step 2).

2. When you first try to use a dev container (Step 5), VS Code will prompt you to install Docker Desktop if it's not detected.

3. Click "Install Docker Desktop" in the notification and follow the automated installation.

4. VS Code will handle the WSL2 integration configuration automatically.

5. Restart your computer when prompted.

**Note**: Option B is more streamlined and should be easier for most users, while Option A gives you more control over the installation process.

## Step 4: Set Up X11 Forwarding (for GUI applications)

### Option A: Using WSLg (Recommended for Windows 11)

> **ℹ️ Note**:
>
> - **Windows 11**: WSLg is included and provides native GUI support. GUI forwarding is automatically handled by WSLg and no additional X11 configuration is required.
> - **Windows 10**: Check if WSLg is available on your system by running `wsl --version` in PowerShell. If WSLg is available, no additional setup is needed. Otherwise, use Option B below.

### Option B: Using VcXsrv (Required for Windows 10 without WSLg)

1. Download and install VcXsrv from [https://sourceforge.net/projects/vcxsrv/](https://sourceforge.net/projects/vcxsrv/)

2. Launch XLaunch with these settings:
   - Display settings: Multiple windows, Display number: 0
   - Client startup: Start no client
   - Extra settings: Check "Disable access control"

3. In WSL2, add to your `~/.bashrc`:

   ```bash
   export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0.0
   export LIBGL_ALWAYS_INDIRECT=1
   ```

## Step 5: Clone and Run the Project

1. Open WSL2 terminal (Ubuntu):

   ```bash
   cd ~
   git clone <repository-url>
   cd ros2_px4_startup
   ```

2. Open the project in VS Code from WSL:

   ```bash
   code .
   ```

3. When prompted, click "Reopen in Container" or use `Ctrl+Shift+P` → "Dev Containers: Reopen in Container"

## Troubleshooting

### Docker Issues

**Problem**: Docker daemon not running

```bash
# In WSL2 terminal, check Docker status:
sudo service docker status
# If not running:
sudo service docker start
```

**Problem**: Permission denied when running Docker commands

```bash
# Add your user to docker group:
sudo usermod -aG docker $USER
# Restart WSL2:
wsl --shutdown
# Open new WSL2 terminal
```

### X11 Display Issues

**Problem**: GUI applications won't start

```bash
# Check DISPLAY variable:
echo $DISPLAY
# Test X11 forwarding:
xclock  # Should show a clock window
```

**Problem**: "No protocol specified" error

- Restart VcXsrv with "Disable access control" enabled
- Or add to Windows Defender Firewall exceptions

### Performance Issues

**Problem**: Slow container performance

- Increase Docker Desktop memory allocation (Settings → Resources → Advanced)
- Move project files to WSL2 filesystem (`/home/username/`) instead of Windows filesystem (`/mnt/c/`)

### WSL2 Integration Issues

**Problem**: Docker not available in WSL2

- Restart Docker Desktop
- Check WSL Integration settings in Docker Desktop
- Ensure your Ubuntu distribution is listed and enabled

## Windows-Specific Notes

1. **File Performance**: Keep your project files in the WSL2 filesystem (`~/projects/`) rather than Windows filesystem (`/mnt/c/`) for better performance.

2. **Resource Allocation**: Docker Desktop on Windows requires significant resources. Adjust memory and CPU allocation in Docker Desktop settings based on your system capabilities.

3. **Antivirus**: Some antivirus software may interfere with Docker operations. Consider adding Docker Desktop and WSL2 to your antivirus exclusions.

4. **USB Device Access**: USB devices (like flight controllers) can be accessed through WSL2 using tools like `usbipd-win`. See [Microsoft's guide](https://learn.microsoft.com/en-us/windows/wsl/connect-usb) for details.

## Next Steps

Once everything is set up, return to the main [README](../README.md) to continue with the Quick Start guide and usage instructions.