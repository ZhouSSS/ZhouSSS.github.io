---
title: Ubuntu20.04安装MiniKube
categories: [云计算,MiniKube]
tags: [中间件,K8S,MiniKube]
---

## 安装Docker (阿里云镜像)

### 1.卸载Docker旧版本
``` bash 
sudo apt-get remove docker docker-engine docker-ce docker.io
```
### 2.添加阿里云GPG秘钥
``` bash 
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```
### 3.设置存储库
``` bash 
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```
### 4.安装docker
``` bash 
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
### 5.验证
``` bash 
sudo docker version

sudo systemctl status docker，未运行时执行sudo systemctl start docker启动docker

sudo docker run hello-world 运行个容器试下

sudo systemctl enable docker  # 设置开机自启
```
### 6.添加阿里镜像加速
``` bash 
sudo vim /etc/docker/daemon.json
#添加：
{
    "registry-mirrors": [
            "https://docker.211678.top",
            "https://docker.1panel.live",
            "https://hub.rat.dev",
            "https://docker.m.daocloud.io",
            "https://do.nark.eu.org",
            "https://dockerpull.com",
            "https://dockerproxy.cn",
            "https://docker.awsl9527.cn"
      ]
}

sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 安装MiniKube
{% note color:warning [MiniKube中文文档](https://minikube.kubernetes.ac.cn/docs/) %}
{% note color:error [MiniKube中文文档](https://minikube.kubernetes.ac.cn/docs/) %}

### 1.依赖
``` bash 
sudo apt-get install -y apt-transport-https
```
### 2.添加阿里GPG
``` bash 
sudo curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add -
```
### 3.添加阿里apt源
``` bash 
sudo tee /etc/apt/sources.list.d/kubernetes.list <<-'EOF' 
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main 
EOF

sudo apt-get update
```
### 4.安装kubectl
``` bash 
sudo apt-get install -y kubectl
```
### 5.添加用户到docker组
``` bash 
sudo usermod -aG docker $USER && newgrp docker
```
### 6.安装mibikube
``` bash 
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
### 7.启动minikube
``` bash 
minikube start --driver=docker --image-mirror-country=cn --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers --kubernetes-version=1.23.8
```
注：结合docker使用时，k8s版本最好不要用1.24及以上版本，k8s从1.24版本开始不在直接兼容docker，需要安装cri-docker。
为了方便，设置alias
``` bash
sudo vim ~/.bashrc
#添加：
alias minikube.cn="minikube start --image-mirror-country=cn --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers --kubernetes-version=1.23.8"
alias kubectl="minikube kubectl --"
#生效
source ~/.bashrc
```
