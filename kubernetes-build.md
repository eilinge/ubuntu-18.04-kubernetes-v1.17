# kubernetes-build

## base url

https://blog.csdn.net/liukuan73/article/details/83116271

cat  daemon.json

{
    "registry-mirrors": ["https://2b49mscf.mirror.aliyuncs.com"],
    "iptables": false,
    "ip-masq": false,
    "storage-driver": "overlay2",
    "graph": "/home/lk/docker",
    "exec-opts": ["native.cgroupdriver=systemd"]
}

## docker images

$ sudo apt-get install -y kubelet=1.17.0-00 kubeadm=1.17.0-00 kubectl=1.17.0-00

k8s-install.sh

#!/bin/bash
KUBE_VERSION=v1.17.0
KUBE_PAUSE_VERSION=3.1
ETCD_VERSION=3.4.3-0
DNS_VERSION=1.6.5
username=registry.cn-hangzhou.aliyuncs.com/google_containers

images=(kube-proxy:${KUBE_VERSION}
kube-scheduler:${KUBE_VERSION}
kube-controller-manager:${KUBE_VERSION}
kube-apiserver:${KUBE_VERSION}
pause:${KUBE_PAUSE_VERSION}
etcd:${ETCD_VERSION}
coredns:${DNS_VERSION}
    )

for image in ${images[@]}
do
    docker pull ${username}/${image}
    docker tag ${username}/${image} k8s.gcr.io/${image}
    docker rmi ${username}/${image}
done

## minikube

minikube start --driver=none --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers' --kubernetes-version=v1.17.0
