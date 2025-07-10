# ROS2 PX4 Startup Template

A comprehensive development container template for UAV controls and robotics research featuring ROS 2 Jazzy, PX4 SITL simulation, and pre-configured development tools.

## Features

- **ROS 2 Jazzy**: Full ROS 2 installation with development tools
- **PX4 Autopilot**: Latest PX4 firmware with SITL simulation capabilities
- **Micro-XRCE-DDS Agent**: Communication bridge between PX4 and ROS 2
- **Gazebo Integration**: `ros-jazzy-ros-gz` and `ros-jazzy-ros-gz-bridge` for simulation
- **Development Tools**: PlotJuggler, CMake, build tools, and debugging utilities
- **VS Code Extensions**: Pre-configured extensions for ROS, C++, Python, and Docker development

## Prerequisites

- Docker
- Visual Studio Code with Dev Containers extension
- X11 forwarding support (for GUI applications)

## Quick Start

1. Clone this repository:

   ```bash
   git clone <repository-url>
   cd ros2_px4_startup
   ```

2. Open in VS Code:

   ```bash
   code .
   ```

3. When prompted, click "Reopen in Container" or use the Command Palette (`Ctrl+Shift+P`) and select "Dev Containers: Reopen in Container"

4. Wait for the container to build (first time may take 10-15 minutes)

## What's Included

### Software Stack

- **Base**: Ubuntu with ROS 2 Jazzy
- **PX4 Autopilot**: Cloned from main branch with all dependencies
- **Micro-XRCE-DDS Agent**: Built and installed for PX4-ROS 2 communication
- **Gazebo**: ROS-Gazebo bridge for realistic simulation

### Development Tools

- Git, CMake, build-essential
- Python 3 with pip and setuptools
- PlotJuggler for data visualization
- FFmpeg for media processing
- Tree command for directory visualization

### VS Code Extensions

- ROS and Ament tools
- C++ and Python development
- Docker integration
- YAML, XML, and Markdown support
- URDF visualization
- CMake tools
- Code formatting (Uncrustify, Ruff)

## Container Configuration

### Workspace Structure

- **Container workspace**: `/workspaces/uav_control`
- **PX4 location**: `/workspaces/PX4-Autopilot`
- **Source code**: `/workspaces/uav_control/src` (copied from host `./src`)

### Networking & Display

- Host networking enabled for seamless ROS communication
- X11 forwarding for GUI applications
- Wayland display support
- Audio forwarding via PulseAudio

### Security & Permissions

- Runs as `ros` user (non-root)
- Privileged mode for hardware access
- USB device access for flight controllers
- Relaxed security for development

## Usage

### Building ROS Packages

```bash
cd /workspaces/uav_control
colcon build
source install/setup.bash
```

### Running PX4 SITL

```bash
cd /workspaces/PX4-Autopilot
make px4_sitl gazebo-classic
```

### Starting Micro-XRCE-DDS Agent

```bash
MicroXRCEAgent udp4 -p 8888
```

### Connecting to Hardware

USB devices are accessible within the container. Connect your flight controller and use standard PX4/ROS 2 workflows.

## Customization

### Adding ROS Packages

Place your ROS 2 packages in the `src/` directory before building the container, or add them later and rebuild using `colcon build`.

### Installing Additional Software

Modify the [Dockerfile](.devcontainer/Dockerfile) to add more packages or tools as needed.

### VS Code Settings

Customize the development environment by modifying the `customizations` section in [devcontainer.json](.devcontainer/devcontainer.json).

## Troubleshooting

### Display Issues

If GUI applications don't work:

```bash
echo $DISPLAY
xhost +local:docker  # On host system
```

### Permission Issues

If you encounter permission errors:

```bash
sudo chown -R ros:ros /workspaces/uav_control
```

### PX4 Build Issues

Ensure you have enough disk space and memory. PX4 compilation requires significant resources.

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test in the development container
5. Submit a pull request

## Acknowledgments

- [PX4 Autopilot](https://github.com/PX4/PX4-Autopilot) team
- [ROS 2](https://ros.org/)
