---
title: setup a gitlab code repository with docker
date: 2020-10-15 19:11:54
categories:
  - coding
tags: git
---
The GitLab Docker images are monolithic images of GitLab running all the necessary services in a single container. If you instead want to install GitLab on Kubernetes, see [GitLab Helm Charts](https://docs.gitlab.com/charts/).

Find GitLab’s official Docker image at:
  - [GitLab Docker image in Docker Hub](https://hub.docker.com/r/gitlab/gitlab-ee/)

## Installation
### Installation GitLab using Docker Engine

You can fine tune these directories to meet your requirements. Once you’ve set up the GITLAB_HOME variable, you can run the image:
```bash
export GITLAB_HOME=/data

sudo docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  gitlab/gitlab-ee:latest
```
This will download and start a GitLab container and publish ports needed to access SSH, HTTP and HTTPS. All GitLab data will be stored as subdirectories of $GITLAB_HOME. The container will automatically restart after a system reboot.

> [GitLab Docker images](https://docs.gitlab.com/omnibus/docker/)

