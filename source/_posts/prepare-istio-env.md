---
title: prepare a devlopment environment for istio
date: 2019-02-20 14:00:00
categories:
  - k8s
  - minikube
  - kubernetes
  - istio
  - sofa-mesh
  - sofa-mosn
tags:
  - k8s
  - kubernetes
  - istio
  - sofa-mesh
  - sofa-mosn
---

In this post, we will describe the process of preparing a development environment for sofa-mesh & sofa-mosn.

## minikube

### install [minikube](https://github.com/kubernetes/minikube)

```bash
mkdir -p $GOPATH/src/k8s.io
git clone https://github.com/kubernetes/minikube.git $GOPATH/src/k8s.io/minikube
cd $_
make
```
[build guide](https://github.com/kubernetes/minikube/blob/master/docs/contributors/build_guide.md)

### start minikube
```bash
#!/bin/bash
#
# start-minikube.sh
# Copyright (C) 2019 liugang.zlg <liugang.zlg@antfin.com>
#
# minikube start script
#

# check parameter
if [ $# -ne 1 ]; then
    echo "Usage: $0 BUILD_ID"
    exit 1
fi

# constants
MINIKUBE_CACHE_DIR=$HOME/.minikube/cache
CURRENT_CONTEXT=minikube-$1
WORKING_DIR=`pwd`/target/$CURRENT_CONTEXT

CURRENT_DRIVER=kvm2
KUBERNETES_VERSION=v1.12.4
REGISTRY_MIRROR=https://uzs7j6l5.mirror.aliyuncs.com

# setup working dirctory
function setUpDir {
    mkdir -p $WORKING_DIR/.kube  $WORKING_DIR/.minikube/cache/iso

    # cache
    cp -rf $MINIKUBE_CACHE_DIR/$KUBERNETES_VERSION $WORKING_DIR/.minikube/cache/
    # cp     $MINIKUBE_CACHE_DIR/iso/minikube-`minikube version|awk '{print $NF}'`.iso $WORKING_DIR/.minikube/cache/iso/
}

# create minikube
function setUpMiniKube() {

    export KUBECONFIG=$WORKING_DIR/.kube/config
    export MINIKUBE_HOME=$WORKING_DIR

    # config cache
    minikube config set cache 'istio/proxyv2:1.1.0-snapshot.4,ubuntu:xenial,istionightly/base_debug:latest,prom/prometheus:v2.3.1'

    # 启动 minikube
    # --registry-mirror: docker.io 在国内访问困难，我们给 docker daemon 指定一个镜像
    # --kubernetes-version: minikube 支持的 k8s 版本非常丰富，可使用命令 minikube get-k8s-versions 获取所有支持的版本
    # --profile: 这个参数非常重要，正是它支撑一台宿主机起多个 minikube实例
    # --vm-driver: 指定虚拟机 driver
    # --bootstrapper: 官方推荐使用 kubeadm 启动 k8s
    minikube start --memory=8192 --cpus=4 --disk-size=30g  \
            --registry-mirror=$REGISTRY_MIRROR \
            --kubernetes-version=$KUBERNETES_VERSION \
            --profile=$CURRENT_CONTEXT \
            --vm-driver=$CURRENT_DRIVER \
            --bootstrapper=kubeadm \
	    --docker-opt "bip=192.168.4.1/24" \
	    --iso-url "file://$MINIKUBE_CACHE_DIR/iso/minikube-`minikube version|awk '{print $NF}'`.iso" \
            -v 5
            # --cache-images \

    minikube profile $CURRENT_CONTEXT

    # 实践中发现 k8s 集群有时候会设置 master node 不允许调度，我们把污点去除
    # kubectl taint nodes --all node-role.kubernetes.io/master-
}

# print minikube info
function echoMiniKube() {
    echo 'minikube ip':`minikube ip`
    echo 'minikube status:'`minikube status`
    echo 'minikube config get profile:'`minikube config get profile`
    echo 'which kubectl:'`which kubectl`
    echo 'kubectl cluster-info:'`kubectl cluster-info`
}

# setup minikube environment
function setUpEnv() {

    cat > $WORKING_DIR/minikube-env << EOF
export MINIKUBE_HOME="$WORKING_DIR"
export KUBECONFIG="$WORKING_DIR/.kube/config"
eval \$(minikube docker-env)
source <(kubectl completion bash)
EOF

    source $WORKING_DIR/minikube-env
}

setUpDir
setUpMiniKube
echoMiniKube
setUpEnv

for i in {1..150}; do
   kubectl get po &> /dev/null
   if [ $? -ne 1 ]; then
      break
  fi
  sleep 2
done

```

