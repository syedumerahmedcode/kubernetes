# Kubernetes

## Video source:
https://www.youtube.com/watch?v=X48VuDVv0do&ab_channel=TechWorldwithNana

Timestamp: 39:47

## Kuberenetes
- K8s is a open source container orchestration tool.
- It helps you _manage containerized applications_(Container are mostly created in docker but it can be any other container technology).
- K8s helps in managing containers in different _deployment_ environments. 
- - It can be physical, virtual, cloud or even hybrid environemnts.

### What problems does k8s solve?

- Trend from _Monolith_ to _Microservices_.
- This resulted in a _increased usage of containers_.
- Demand of a proper way of _managing_ those hundreds of containers.

### What are the tasks of an orchestration tool?

- _High availability_(or no downtime)
- _Scalibility_(Scalable web applications are capable of handling increased user traffic and growing demands without compromising performance)
- _Disaster recovery_(backup and restore)

## Kubernetes Components

### Node
- A simple server - a physical or virtual machine.

### Pod
- Smallest unit K8s.
- **In short:** It is an abstraction over the container.
- -  Note: We do not work with the underlying container technology but rather we only interact with the Kubernetes layer.
- Usually, one application runs per pod. 
- Each pod gets its own IP address(_not the container but the pod itself gets an IP address_).
- Please note that pods are ephemeral ion nature i.e. they can die very easily. When that happens, a new pod is created which takes place of the recently deceased pod.
- New IP address on re-creation of a pod.
![pod](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/pod.png)

### Services
- This creates a permenant static IP address for the pod.
- **In short:** It is used for communication between pods.
- The lifecycles of the service and the pod are **not** connected. This means that if the pod dies, the service and its IP address will remain.This means that the endpoint remains the same.
- Service also acts as a **load balancer** meaning that the service will catch the request coming from the ouside via Ingress(see below) and forward to the pod which is least busy.
![service](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/service.png)

### Ingress
- Application is made accessible via a browser using ingress.
- **In short:** Routes traffic into cluster.
- External service is a service that opens the communication to external sources. It is a http IP address of the K8s service and the IP address of the said K8s service.
- Ingress removes the IP address of the K8s service and instead creates a user readable(user friendly) web address. 
- After creating an ingress, the user request first goes to ingress and then it goes to the service.
![ingress](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/ingress.png)

### ConfigMaps
- **In short:** It contains the external configuration to the application. For example, database name. 
- - **Advantage:** Without configmap(and K8s), if database name chnaged, one would have to rebuild the docker image, push it to docker repository and then pull the image. With configmap, all of these extra steps are not needed anymore.
- Once it is connected to the pod, the pod can see and read the values in the configmap.
![configmap](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/configmap.png)


### Secrets
- **In short:** Used for storing secret data.
- The credentails(or other secret data)stored is base64 encoded.
- Once it is connected to the pod, the pod can see and read the values in the secret.
![secret](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/secret.png)


### Volumes
- Volume attaches a physical storage on a hard drive to your pod.
- **In short:** It takes care of data persistence.
- The storage can be on a local machine or on a remote machine which is outside of the K8s cluster.
- Please note that K8s explicity _does not_ manage any data persistence.
![volume](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/volume.png)

### Deployment
- Replicate everything.
- Here, we define a **blueprint of the pod**(for example, _my-app_ pod) and specify **how many replicas** of that pod you would like to run. That means, we can sclae up or scale down the umber of replicas. 
- **In short:** pod blueprint with replicating mechanisms.
- Hence, we define the _blueprint_ of the underlying pods.
- We created _deployments_ and _in practice, we mostly work with deployments, not pods_. As:
- Deployment is an _abstraction_ on pod just like pod is an _abstraction_ on the container.
![deployment](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/deployment.png)




### Statefulset
- Databases _cannot_ be replicated via Deployment as it **has a state** (its data). 
- Statefulset is meant for **Stateful apps** such as MySql, MongoDb, Elastic search, MS SQL Server etc.
- **In short:** Statefulset makes sures to replicate the stateful applications (in terms of number of instatnce) along with the correct state.
- Please note that deploying statefulset in K8s is _not_ easy(as in it is more difficult when comapring to working with deployments). Hence, normally, databases are stored outside of K8s cluster.

- **Note:** _Deployment_ is meant for **stateless applications** and _Stateful_ is meant for **Stateful apps or databases**.

![statefulset](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/statefulset.png)


### Attention
- All images of various K8s components are taken at various timestamps from the video in the video source.


## K8s Architecture

### Node processes
- Worker machine in K8s cluster.
- Each node has multiple parts on it.
- 3 processes must be installed on every node.
- Worker nodes do the actual work.
![workernode](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/workernode.png)
- - Container runtime: Normally docker but it  can be some other technology. It is needed as containers are running on every node hence a container runtime is needed.
- - Kubelet: the process that schedules the pods and container underneath. Kubelet interacts with both the (docker)_container_ and the _node_. Kubelet starts a pod with a container inside. Kubelet also assigns resources(RAM, CPU, storage etc.) from the node to the container.
![kubelet](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/kubelet.png)
- - Kube proxy: kubeproxy forwards the requests in an intelligent manner that amkes sure that the communication also works in a performant manner with low overhead. [Diagram at 26:00?

### How do we interact with K8s cluster?

#### How to schedule pod?


#### How to monitor?


#### How to reschedule/restart pod?


#### How to join a new Node?

All of the **how to** questions listed above are handled by Master node.

- **4 processes run** on every master node. These are:

--> **API server**: API server is _like a cluster gateway_. It also acts a _gate keeper for authentication_.

![apiserver](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/apiserver.png)

API server also acts a single entrypoint into the cluster.

--> **Scheduler**: After the request is validated by the API server, it forwards the requests to the scheduler which will start the pod on one of the worker nodes. Please mote that the scheduler _just decides_ on which Node new Pod should be scheduled. The _process of actually creating a pod_ on the worker node is done by _kubelet_.

![scheduler](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/scheduler.png)

--> **Controller manager**: Controller manager _detects state changes_ in the cluster such as pods dying.So, when the pod dies, the controller manager detects that and tries to recover the cluster state as soon as possible.

![controllermanager](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/controllermanager.png)

--> **etcd**: This is a _key value stored_ of the cluster state. etcd is the cluster brain as in every cluster change gets stored in the key value store of etcd.
Please note that the actual application data is not stored in the etcd. 

![etcd](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/etcd.png)


### Minikube and Kubectl

#### Minikube

In a production cluster setup, one has at least two master nodes and three or more worker nodes.These nodes exist on separate virtual or physical machines. To setup this sort of setup on a local machine is quiet difficult. Hence, _minikube_ works as a good compromise.

**minikube** is a one node cluster which master processes and worker processes both run on _one node(machine)_. Minikube also has docker container runtime pre-installed.

Minikube runs via a _virtual box(or some other hypervisor)_ on the local machine. So,

- creates virtual box on the laptop
- node runs in that virtual box
- It is a 1 node K8s cluster.
- It is primarily for testing purposes.

![minikube](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/minikube.png)

- How to install minuikube on linux?

https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download


To install the latest minikube **stable** release on **x86-64 Linux** using binary download:
~~~
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
~~~

From a terminal with administrator access (but not logged in as root), run:
~~~
minikube start
~~~


#### kubectl

- kubectl is a command line tool for K8s cluster.
- Kubectl talks with Apiserver(within the master process) located within minikube.
- Using kubectl, one can do anything within the K8s cluster such as create pods, destroy pods, create services etc.

![kubectl](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/kubectl.png)

- How to install Kubectl on linux?

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
