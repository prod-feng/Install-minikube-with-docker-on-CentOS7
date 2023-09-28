# Install-minikube-with-docker-on-CentOS7
This following steps list the way I installed minikube with docker on CentOS 7.

1) Install Docker-ce

First, remove any old docker package installed.Add the docker-ce repo and then install.
```
yum remove docker docker-common
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install  docker-ce
```

Make sure dcoker and contanerd serverices are running:
```
  systemctl status docker
  systemctl status containerd
```

2) Install minikube

There are very good instructions here(https://minikube.sigs.k8s.io/docs/start/). I list all the commands together here:

As root:
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
groupadd docker(should be setup when install docker, in case not)
usermod -aG docker feng
```

As regular user to test(may need to install kubectl package):
```
minikube start --driver=docker
kubectl get po -A
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
kubectl get services hello-minikube
......
minikube stop
minikube delete --all
```

Note: Rootless Docker only works on kernel 5.11 or later, like Redhat 9,ubuntu 22.
