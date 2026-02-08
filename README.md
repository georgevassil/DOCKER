# Docker Cheat Sheet & ROS Setup

This repository contains a quick reference for essential Docker commands and a specific configuration for running ROS (Robot Operating System) containers with GUI and GPU support.

## 🐳 Basic Commands

Displays the currently active (running) containers:
```bash
docker ps
```

Displays all containers, including those that are inactive or stopped:
```bash
docker ps -a
```

Downloads (pulls) the specified image from Docker Hub:
```bash
docker pull docker/welcome-to-docker
```

Lists all the Docker images currently downloaded on your machine:
```bash
docker image ls
```

Shows the history of layers for a specific image:
```bash
docker image history docker/welcome-to-docker
```

---

## 🔄 The Docker Workflow

The standard lifecycle: Build → Run → Check → Stop → Delete.

Creates (builds) an image from a Dockerfile (the recipe) in the current directory:
```bash
docker build -t <name> .
```

Starts the application in the background and maps port 3000 to 3000:
```bash
docker run -dp 3000:3000 <name>
```

Checks to see what is currently running:
```bash
docker ps
```

Stops a specific container using its ID:
```bash
docker stop <id>
```

Removes (deletes) a specific container using its ID:
```bash
docker rm <id>
```

---

## 🤖 Advanced Usage: ROS with GUI & GPU Support

Use the following configuration to run ROS Noetic with full desktop support.

### The Full Command
This command runs the container with a custom name, display forwarding, GPU access, and network sharing:
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

**--name**
Assigns a custom name so you don't have to use a random ID:
```bash
--name ros1_talos2
```

**-e**
Sets the environment variable to forward the host display to the container:
```bash
-e DISPLAY=$DISPLAY
```

**--gpus**
Grants the container access to all available GPUs (requires NVIDIA Container Toolkit):
```bash
--gpus all
```

**-v**
Mounts the X11 socket volume to allow the GUI to connect to your screen:
```bash
-v /tmp/.X11-unix:/tmp/.X11-unix:rw
```

**-it**
Allocates a pseudo-TTY and keeps stdin open (interactive mode):
```bash
-it
```

**--net**
Uses the host's network stack directly (simplifies ROS port management):
```bash
--net=host
```

**Image**
The specific ROS Noetic image to use:
```bash
osrf/ros:noetic-desktop-full
```
