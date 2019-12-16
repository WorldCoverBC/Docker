# Synopsis

This repository includes Docker files to create Docker images for the Worldcover Sentinel-2
pre-processing steps. To create Docker images from Docker files, you have to install Docker
Engine. Instructions for setting up Docker are given below.

# Instructions for setting up Docker

## Linux

The following instructions are for Ubuntu Linux. [Instructions for other Linux systems](https://docs.docker.com/install/) can be found on the [Docker web site](https://docs.docker.com/). Open a terminal window and type the following commands

    sudo apt-get update

    sudo apt-get install \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg-agent \
      software-properties-common

Add Docker's official GPG key

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

Verify that you now have the fingerprint

    sudo apt-key fingerprint 0EBFCD88

Type the following command to add the stable Docker repository

    sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"

Install Docker Engine

    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io

## Linux post-installation steps (optional)

Create  the `docker` group, if it does not exist already

    sudo groupadd docker

Add your user to the `docker` group

    sudo usermod -aG docker $USER

Type

    newgrp docker

to activate the changes to groups. Then type

    docker run hello-world

to verify that Docker Engine is installed correctly and you can use the
docker command without `sudo`.

## Windows and WSL

Follow the instructions given in [this blog](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly). In brief

1. Install Docker Desktop
2. Start Docker Desktop
3. Edit the settings of Docker Desktop to expose the Docker daemon on `tcp://localhost:2375` (Last checkbox "General" Tab)
4. Enable WSL
5. Install Ubuntu Linux from the Microsoft Store
6. Start WSL/Ubuntu
6. Follow the instructions above to install Docker Engine on WSL/Ubuntu
7. Add the following line to your `.profile`

    export DOCKER_HOST="tcp://localhost:2375"

8. Type the following line to test your setup

    docker run hello-world
    
9. Adjust the memory available to Docker Engine in the settings of Docker Desktop 

The Docker Engine installed on WSL/Ubuntu will then use the Docker daemon running on Windows.

## Windows

You may use Docker Desktop for Windows without WSL. If you want to base your Docker images onto Linux, however, it is of advantage to test the commands you `RUN` in your Dockerfile beforehand (for which you may use WSL).
