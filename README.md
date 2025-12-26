# Containerized Linux Browser Environments

This project provides a set of containerized Linux distributions (currently Alpine) that run a desktop environment accessible via a web browser using noVNC.

## Features

- **Alpine Linux**: Lightweight distro with a graphical interface.
- **Browser Access**: Connect to the desktop interface directly from your browser.
- **Dockerized**: Easy deployment using Docker Compose.

## Prerequisites

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Installation

1.  Clone the repository:
    ```bash
    git clone <repository-url>
    cd linux_browser
    ```

2.  Start the containers:
    ```bash
    docker-compose up -d
    ```

## Usage

### Alpine Environment

- **URL**: [http://localhost:6081](http://localhost:6081)
- **User**: `azuriieal`
- **Password**: `1234`
- **VNC Password**: `123456`

## Configuration

The environment variables and ports can be configured in the `docker-compose.yml` file.

| Variable        | Description | Default |
| :---            | :---        | :---    |
| `USER_NAME`     | Username    | `azuriieal` |
| `USER_PASSWORD` | Password    | `1234`  |
| `VNC_PASSWORD`  | VNC Password| `123456`|

## Architecture & Design Philosophy

This project follows a modular "layer cake" approach to containerizing desktop environments. The goal is to keep images lightweight while providing a full graphical experience.

### Why these technologies?

- **Alpine Linux**: Chosen as the base for the first environment because it is extremely lightweight (security-oriented and small footprint), making it ideal for quick container startups.
- **XFCE4**: A lightweight desktop environment that is stable and resource-friendly, perfect for containers where resources might be limited compared to Heavy DEs like GNOME or KDE.
- **TigerVNC / noVNC**: VNC is the standard for remote desktop access. noVNC provides a seamless HTML5 web client, removing the need for users to install a native VNC viewer.

### How It Works

The stack is built in the following layers:

1.  **Base Layer (OS)**: starts with a minimal Linux image (e.g., `alpine:latest`).
2.  **GUI Layer**: Installs the X11 window system and a Desktop Environment (e.g., XFCE).
3.  **VNC Layer**: Sets up a VNC server (like `vncserver` or `x11vnc`) to capture the X11 display.
4.  **Web Access Layer**: Runs `noVNC`, which acts as a proxy, translating VNC (RFB protocol) to WebSockets so you can view it in a browser.

## Extending to Other Distributions

You can replicate this setup for other distributions (Ubuntu, Kali, Arch) by following the same pattern:

1.  **Create a Directory**: `mkdir -p distros/your-distro`
2.  **Create Dockerfile**:
    - `FROM` your base image (e.g., `ubuntu:latest`).
    - **Install**: Update repos and install your DE (e.g., `xfce4`, `mate`, `lxde`) and VNC server (`tigervnc-standalone-server` or `tightvncserver`).
    - **Configure**: Set up the VNC password and startup scripts.
    - **Entry**: Ensure the container keeps running (often by keeping the VNC server in the foreground or tailing a log).
3.  **Update Compose**: Add a new service in `docker-compose.yml` pointing to your new directory.

This modular structure allows for infinite extensibility to any Linux environment you wish to containerize.

