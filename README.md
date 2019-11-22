# Synopsis

This repository includes Docker files to create Docker images for the Worldcover Sentinel-2
pre-processing steps. To create Docker images from Docker files, you have to install Docker
Engine. Instructions for installing Docker Engine on Ubuntu Linux are given below.

# Installation of Docker Engine

Open a terminal window and type the following commands

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

# Post-installation steps (optional)

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
