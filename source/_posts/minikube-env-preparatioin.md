---
title: prepare a minikube environment
date: 2019-01-22 15:00:00
categories:
  - k8s
  - minikube
tags:
  - k8s
  - minikube
---

### Install go language

# install older golang, because there must be go command during compile golang source code
sudo yum install -y golang
# get golang source
mkdir -p ~/git && git clone https://github.com/golang/go.git ~/git/go
cd ~/git/go/src
# compire source code
./all.bash
# remove older golang
sudo yum remove -y golang

### Install Docker-CE

### Install minikube
