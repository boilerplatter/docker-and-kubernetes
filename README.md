# docker-and-kubernetes

KU ACM workshop
2020-02-05
Lawrence, KS

(I will push additional exercises to this GitHub repository in advance of the workshop, so bookmark it!)

This workshop is for all CS students and anyone who enjoys making software and wants to learn common industry tools for doing so effectively. Bring a computer along to maximize your learning, ideally with 8GB or more of RAM (because Docker is hungry just like the 🐳 in its logo implies) and the following steps taken care of a day or two ahead of time:

## Preparation

Some large downloads are involved: do these steps in advance!

1. Install Docker
    - [Mac](https://download.docker.com/mac/stable/Docker.dmg)
    - [Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)
    - [Linux](https://docs.docker.com/install/#server)

1. Install Kubernetes
    - [Mac (already installed with Docker; just enable)](https://docs.docker.com/docker-for-mac/#kubernetes)
    - [Windows (already installed with Docker; just enable)](https://docs.docker.com/docker-for-windows/#kubernetes)
    - [Linux (recommend microk8s)](https://microk8s.io/#get-started)

1. Prime your local Docker cache (to not burn down JAYHAWK wifi during workshop)

    ```bash
    # Run this command in bash in your terminal
    for image in bash gcc python:3.7-slim
    do
        docker pull "$image"
    done

    # I have no Windows machine or partition to test in right now, but the following might work there:
    for %i in (bash gcc python:3.7-slim) do docker pull %i

    # Please submit a PR to fix that command if it's broken :)
    ```

1. Clone this repository locally

    ```bash
    git clone https://github.com/boilerplatter/docker-and-kubernetes.git

    # or if you have SSH keys set up with GitHub:
    git clone git@github.com:boilerplatter/docker-and-kubernetes.git
    ```

1. (optional) Install VS Code

   I will be running demos in VS Code, and having it installed locally will allow you to exactly duplicate the screen layout I use, and acquire some handy Dockerfile and Kubernetes/YAML plugins en route.

   [Download from visualstudio.com](https://code.visualstudio.com/download)

## Goals

- 📦 Learn what a container is
- 🛠️ Understand what
  - container technologies like Docker, and
  - clustered container orchestration technologies like Kubernetes

  help real-world teams of developers and operators do
- ⚖️ Discuss some of the tradeoffs that are involved (there are always tradeoffs!)
- 🎽 Gain experience creating and running containers with Docker, and managing them in Kubernetes
  - (this is very relevant experience for many internships and jobs)
- 🗺️ Know ~~the right words to google~~ how to keep learning after this workshop
