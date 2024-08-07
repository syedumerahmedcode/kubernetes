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

### Service

Create a sample deployment and expose it to port:8080

~~~
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
~~~

Your deployment will show up when you run:

~~~
kubectl get services hello-minikube
~~~

Let minikube launch a browser to access this service:

~~~
minikube service hello-minikube
~~~

Use kubectl to forward the port:

~~~
kubectl port-forward service/hello-minikube 7080:8080
~~~

The application is now available at http://localhost:7080/

### LoadBalancer

To access a LoadBalancer deployment, use the “minikube tunnel” command. Here is an example deployment:

~~~
kubectl create deployment balanced --image=kicbase/echo-server:1.0
kubectl expose deployment --type=LoadBalancer --port=8080
~~~

In a separate window, start the tunnel to create a routable IP for the 'balanced' deployment.

~~~
minikube tunnel
~~~

To find the routable IP, run this command and examine thhis external IP column.

~~~
kubectl get services balanced
~~~

Your deployment is now available at <EXTERNAL-IP>:8080

### Ingress

Enable ingress addon:

~~~
minikube addons enable ingress
~~~

The following example creates simple echo-server services and an Ingress object to route to these services.

~~~
kind: Pod
apiVersion: v1
metadata:
  name: foo-app
  labels:
    app: foo
spec:
  containers:
    - name: foo-app
      image: 'kicbase/echo-server:1.0'
---
kind: Service
apiVersion: v1
metadata:
  name: foo-service
spec:
  selector:
    app: foo
  ports:
    - port: 8080
---
kind: Pod
apiVersion: v1
metadata:
  name: bar-app
  labels:
    app: bar
spec:
  containers:
    - name: bar-app
      image: 'kicbase/echo-server:1.0'
---
kind: Service
apiVersion: v1
metadata:
  name: bar-service
spec:
  selector:
    app: bar
  ports:
    - port: 8080
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
    - http:
        paths:
          - pathType: Prefix
            path: /foo
            backend:
              service:
                name: foo-service
                port:
                  number: 8080
          - pathType: Prefix
            path: /bar
            backend:
              service:
                name: bar-service
                port:
                  number: 8080
---
~~~

Apply the contents

~~~
kubectl apply -f https://storage.googleapis.com/minikube-site-examples/ingress-example.yaml
~~~

Wait for ingress address

~~~
kubectl get ingress
NAME              CLASS   HOSTS   ADDRESS          PORTS   AGE
example-ingress   nginx   *       <your_ip_here>   80      5m45s
~~~

**Note for Docker Desktop Users:**

To get ingress to work you’ll need to open a new terminal window and run minikube tunnel and in the following step use 127.0.0.1 in place of <ip_from_above>.

Now verify that the ingress works

~~~
$ curl <ip_from_above>/foo
Request served by foo-app
...

$ curl <ip_from_above>/bar
Request served by bar-app
...
~~~

