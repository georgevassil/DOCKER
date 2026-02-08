# Docker Cheat Sheet & ROS Setup

This repository contains a quick reference for essential Docker commands and a specific configuration for running ROS (Robot Operating System) containers with GUI and GPU support.

## 🐳 Basic Commands

### Container Status
Check the status of your containers.

```bash
# List currently running (active) containers
docker ps

# List ALL containers (active and inactive/stopped)
docker ps -a
```

### Image Management
Commands to manage Docker images.

```bash
# Download (pull) an image from Docker Hub
docker pull docker/welcome-to-docker

# List all downloaded images on your machine
docker image ls

# View the layers of a specific image
docker image history docker/welcome-to-docker
```

---

## 🔄 The Docker Workflow

The standard lifecycle of a containerized application: **Build → Run → Manage → Clean.**

| Action | Command | Description |
| :--- | :--- | :--- |
| **Build** | `docker build -t <name> .` | Creates an image from a Dockerfile ("The Recipe"). |
| **Run** | `docker run -dp 3000:3000 <name>` | Starts the application. <br>`-d`: Detached mode (background). <br>`-p`: Maps host port to container port. |
| **Check** | `docker ps` | Verifies the container is running. |
| **Stop** | `docker stop <container_id>` | Stops a running container. |
| **Delete** | `docker rm <container_id>` | Removes the container file system. |

---

## 🤖 Advanced Usage: ROS with GUI & GPU Support

The following command is used to run a ROS Noetic container with full desktop support, NVIDIA GPU acceleration, and X11 forwarding (to view graphical interfaces like Rviz or Gazebo).

### The Command

```bash
docker run --name ros1_talos2 \
  -e DISPLAY=$DISPLAY \
  --gpus all \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -it \
  --net=host \
  osrf/ros:noetic-desktop-full
```

### Flag Breakdown

| Flag | Explanation |
| :--- | :--- |
| `--name ros1_talos2` | **Custom Name:** Assigns a specific name to the container so you don't have to rely on the random ID. |
| `-e DISPLAY=$DISPLAY` | **Environment Variable:** Passes the host's display ID (e.g., `:0` or `:1`) to the container, allowing it to know where to send graphical output. |
| `--gpus all` | **GPU Access:** Grants the container access to all available GPUs (requires NVIDIA Container Toolkit). |
| `-v /tmp/.X11-unix...` | **Volume Mount:** Mounts the X11 socket from the host to the container. This creates the physical connection required for the GUI to render on your screen. |
| `--net=host` | **Network:** Uses the host's network stack directly. Since ROS uses many random ports for communication, this is easier than mapping ports individually. |
| `-it` | **Interactive TTY:** Allocates a pseudo-TTY and keeps stdin open, allowing you to interact with the command line inside the container. |
| `osrf/ros:noetic...` | **The Image:** The specific Docker image to run. |

### Optional Startup Command
You can append a command at the very end of the `docker run` string to execute it immediately upon startup.
*Example:* Adding `nvidia-smi` at the end would print GPU statistics and then exit.
