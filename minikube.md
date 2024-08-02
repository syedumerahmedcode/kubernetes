# Minikube

## Installation

To install the latest minikube **stable** release on **x86-64 Linux** using binary download:
~~~
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
~~~

From a terminal with administrator access (but not logged in as root), run:
~~~
minikube start
~~~

## Start your cluster

From a terminal with administrator access (but not logged in as root), run:

~~~
minikube start
~~~

## Interact with your cluster

If you already have kubectl installed (see documentation), you can now use it to access your shiny new cluster:

~~~
kubectl get po -A
~~~

Alternatively, minikube can download the appropriate version of kubectl and you should be able to use it like this:

~~~
minikube kubectl -- get po -A
~~~

minikube bundles the Kubernetes Dashboard, allowing you to get easily acclimated to your new environment:

~~~
minikube dashboard
~~~

## Deploy applications

