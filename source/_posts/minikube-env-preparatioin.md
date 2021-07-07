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

Prepare a environent with golang, docker-ce, minikube installed. We will begin develop istio in this environment.

### Install go language
```bash
# install older golang, because there must be go command during compile golang source code
sudo yum install -y golang
# get golang source
mkdir -p ~/git && git clone https://github.com/golang/go.git ~/git/go
# compire source code
cd ~/git/go/src; ./all.bash
# remove older golang
sudo yum remove -y golang
```

### Install Docker-CE
```bash
# Step 1: add aliyun and docker-ce repository
sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sudo dnf config-manager --add-repo  https://download.docker.com/linux/centos/docker-ce.repo
sudo yum makecache

# Step 2: install docker-ce (we should set http(s)_proxy because of GFW)
sudo yum install -y docker-ce

# Step 3: start docker daemon
sudo systemctl enable docker && sudo systemctl start docker

# Step 4: add current user to docker group
sudo usermod -a -G docker $(whoami) && newgrp docker

# (Optional) Step 5: test docker
docker run hello-world

```

### Install docker-machine
```bash
base=https://github.com/docker/machine/releases/download/v0.16.1 &&
  curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
  sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

### Install VirtualBox(https://www.virtualbox.org/wiki/Linux_Downloads)
[doc](https://wiki.centos.org/HowTos/Virtualization/VirtualBox)
```bash
# Step 1: download virtualbox
curl -Lo VirtualBox-6.0-6.0.2_128162_el7-1.x86_64.rpm https://download.virtualbox.org/virtualbox/6.0.2/VirtualBox-6.0-6.0.2_128162_el7-1.x86_64.rpm

# Step 2: install virtualbox
sudo yum install VirtualBox-6.0-6.0.2_128162_el7-1.x86_64.rpm

# Step 3: install vboxdrv kernel module
sudo /usr/lib/virtualbox/vboxdrv.sh setup

# Step 3: test create a virtual machine
docker-machine create -d virtualbox --virtualbox-disk-size "100000" large

```

### Install kvm driver
```bash
# CentOS environment 
sudo yum install qemu-kvm qemu-img virt-manager libvirt libvirt-python libvirt-client virt-install virt-viewer bridge-utils libguestfs-tools 
sudo usermod -a -G libvirt $(whoami)
newgrp libvirt

# Ubuntu 16.04
sudo apt install qemu-kvm libvirt-bin
sudo usermod -a -G libvirtd $(whoami)
newgrp libvirtd

# ubuntu 18.04
sudo apt install libvirt-clients libvirt-daemon-system qemu-kvm
sudo usermod -a -G libvirt $(whoami)
newgrp libvirt

# kvm2 driver
curl -Lo docker-machine-driver-kvm2 https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-kvm2 \
&& chmod +x docker-machine-driver-kvm2 \
&& sudo cp docker-machine-driver-kvm2 /usr/local/bin/ \
&& rm docker-machine-driver-kvm2

# list virtual machine
virsh list --all
```

### Install minikube
We will install minikube in virturebox
```bash
# Step 1: download minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo cp minikube /usr/local/bin/ && rm minikube

# Step 2: download kubectl
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo cp kubectl /usr/local/bin/ && rm kubectl

# Step 3: start minikube
REGISTRY_MIRROR=https://docker.mirrors.ustc.edu.cn
KUBERNETES_VERSION=v1.12.4
CURRENT_CONTEXT=minikube-`date +%s`
CURRENT_DRIVER=kvm2
minikube start --memory=8192 --cpus=4 --disk-size=30g \
                --registry-mirror=$REGISTRY_MIRROR \
                --kubernetes-version=$KUBERNETES_VERSION \
                --profile=$CURRENT_CONTEXT \
                --vm-driver=$CURRENT_DRIVER \
                --bootstrapper=kubeadm
   
minikube profile $CURRENT_CONTEXT
```



