# Kubernetes

## Video source:
https://www.youtube.com/watch?v=X48VuDVv0do&ab_channel=TechWorldwithNana

Timestamp: 1:57:42

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

Further notes in minikube.md.

- minikube has docker runtime pre-installed so even if docker is not installed on one's local machine, it is still going to work.


#### kubectl

- kubectl is a command line tool for K8s cluster.
- Kubectl talks with Apiserver(within the master process) located within minikube.
- Using kubectl, one can do anything within the K8s cluster such as create pods, destroy pods, create services etc.

![kubectl](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/kubectl.png)

- How to install Kubectl on linux?

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

**Minikube CLI** for startup and dleeting the cluster
whereas
**Kubectl CLI** for configuring the minikube cluster.

## K8s Configuration YAML file
- The 3 parts of a configuration file
- Connecting _deployment_ to _services_ to _pods_.

### 3 parts of a configuration file

- metadata
- specification: Attributes are specific to the 'kind' of the component.
- status: Desired vs Actual--> This is slef-checking/self-healing feaure that Kubernetes provides. K8s updates status continuously! Etcd holds the current status of any K8s component.

### Connecting deployment to pod

- metadata part contains the labels. 
- - any key-value pair for component.
- - pods gets the label through the template blueprint.
- specification part contains selectors.
- -  The label (of pods) is matched to the selctor.
- - The _app_ under _selector_ of _service_ searches for _app_ under _label_ in _deployment_. This way, service knows which pods are registered with it and this connection is amde via the selctor of the label.
![connectingservicestodeployment](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/connectingservicestodeployment.png)


### Ports in services and pod

![portsinservicesandpodone](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/portsinservicesandpodone.png)
![portsinservicesandpodtwo](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/portsinservicesandpodtwo.png)


## Demo Project: MongoDB and MongoExpress

The following diagram gives an overview of kubernetes components in the sample application.
![overviewofk8scomponentsindemoapplication](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/overviewofk8scomponentsindemoapplication.png)

Keeping the above diagram in mind, the browser requestg flow through various kubernetes componenets will look like the following:
![browserrequestflowthroughthekubernetescomponents](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/browserrequestflowthroughthek8scomponents.png)

### How are deployment and secret connected

Genrally speaking, the passwords are not written directly on the configuration file **configmap** as 
- 1) It is part of the repository, and
- 2) The password will be stored in plain text

A better way is to stored the passwords in a separate file called **secrets**. The secrets file contains a set of key value pairs in which the passwords are stored.

#### On the secret file side
Here we define the __name: mongodb-secret__, we use the default type i.e. __type: Opaque__ and here, under the data section we define key-value pairs. For example: _mongo-root-username: dXNlcm5hbWU=(where dXNlcm5hbWU= is the base64 encoded version of 'username')_.

Note:
- The encoding is not enabled by default and one has to stored the encoded value in the key -value pair.

#### On the configmap file side
Here, under the __spec__ section after we define __image__ and __port__ details, we have __env__ variables section. Here, we use **valueFrom** with **secretKeyRef** underneath it and we define the __name: mongodb-secret(whihc is the secret filename)__ and __key: mongo-root-username(meaning which key of the specified secret file is referenced here)__.

K8s will replace the __key__ with the corresponding base64 decoded value once the configmap is applied via kubectl.



The following snapshot describes how a secret is linked to deployment in yaml configurations within kubernetes:
![howsecretislinkedtodeploymentinyamlconfiguration](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/howsecretislinkedtodeploymentinyamlconfiguration.png)

### Service
Here the _kind_ is **service**, _metadata_ is a random name and _selector_is used to **connect to the pod using label (defined in the deployment part of the yaml file).**
In the **ports** parts, we define the _port_ which is the **service port** whereas the _targetPort_ is the **containerport of deplyoment**.
Please note that _taregtPort(of the service.yaml)_ MUST MATCH the _containerPort(of the deployment.yaml)_.

A summary of the bove information is as follows:
![serviceconfigurationfile](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/serviceconfigurationfile.png)

#### How to check if the service is listening to the correct pod?
- First run: __kubectl get service__: This will list all services which are running in k8s cluster.

- Then run __kubectl describe service mongodb-service__: This lists all charateristics of the mongodb-service(the one shihc we created for the demo project). Here, we get **Endpoints:         10.244.0.19:27017** which is _the IP address on which the pod_ and the port is _27017_ on which the application inside the pod is listening.

- now run __kubectl get pod -o wide__: This list all pods running in the k8s cluster and currently, we see the following output: 
__mongodb-deployment-585bb4fddc-hpgsf   1/1     Running   0             45h   **10.244.0.19**   minikube__

Here, one can see that not only the IP address matches with ethe service but also the deployment name is the one which we used in the demo project.

![ipaddressofservicematchesipaddressofthepod](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/ipaddressofservicematchesipaddressofthepod.png)



### Deployment(for mongo-express)
In this file, everything under __template__ is basically __pod definition__. For example; __image: mongo-express__, here image name is mongo-express. Please note that mongo-express listen on port 8081. 

In order to connect it to mongodb, we need to tell it 3 things:
- Which database to connect to? 
- - MongoDB address/internal address-->ME_CONFIG_MONGODB_SERVER

- which credentails to authenticate?
- --> Username: ME_CONFIG_MONGODB_ADMINUSERNAME
- --> Password: ME_CONFIG_MONGODB_ADMINPASSWORD

This is how they are connected:
![mongoexpressenvironmentvariables](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/mongoexpressenvironmentvariables.png)

### ConfigMap(for mongo-express)
The configmap file is very similar to secret in terms of content. Here, we have:
- **kind**: configmap
- **metadata**: a random name
- **data**: the actual contents- in key-value pair form.

![configmapconfigurationfile](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/configmapconfigurationfile.png)

Please note that configmap **must already** be in the K8s cluster when we reference it. 

### How to make it an External service(for mongo-express)
The service part of mongo-express is similar to service yaml fo mongodb. However, in order to make the service externally visible, one needs to do the following two things:

- **type: LoadBalancer**--> By usig this selector attribute, the service assigns an **external IP address** and hence, it accepts external requests. please note that __internal Service or ClusterIP__ is DEAFAULT. LoadBalncer assigns in addition to the cluser-IP an **External-IP**.
- **nodePort: a port number within a special range**--> This is the port for external IP address.That is, this is the port one needs to enter in the browser. Please note that nodeport has a range between __30000 - 32767__ only.

![howtomakeanexternalservice](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/howtomakeanexternalservice.png)

In order to start mongo-express from a browser _when running on minikube_, one can use the following command:

~~~
minikube service mongo-express-service
~~~

once we have signed in to the mongodb express externally, if we __create a new database__, the following steps happen:

Mongo Express external service ---> mongo express pod ---> Mongo DB Internal Service ---> Mongo DB Pod

A graphical representation of this process is shown below:

![stepsfrommongoexpressexternalservicetillmongodbpod](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/stepsfrommongoexpressexternalservicetillmongodbpod.png)


### K8s namespace explained

- **What is a namespace?**
--> one can organise resources in a namespace. Think of it like a virtual cluster inside a cluster. By default, k8s provides 4 namespaces.

**kube-system**: This is meant for the k8s system and it holds system processes such as master and kubectl processes. NOTE: Do not create or modify in kube-system.
**kube-public**: This namesapce is meant for publicly accessible data. It contains a configmap, whihc contains cluster-information. This information can be accessed via the following command: __kubectl clsuetr-info__
**kube-node-lease**: It holds the information about the hearbeats of nodes and it determines the availability of a node.
**default**: The resources we create are located here(provided no custom namesapce is defined in k8s).

Additionally, we have:
**kubernetes-dashboard**: This is only specific with minikube. One would not see this in a standard cluster.

- **What are the use cases?**
- Resources are grouped and provided a better overview when using namespaces. For example 'database' anemspace or 'monitoring' namespace.
- Another use case is when the same application is accessed via multiple teams. Here, alos, having team based namespace is a good way forward. This way, teams can avoid the scenario where the components of first team are (unintentionally) overwritten by other teams.
- One more use case is when Staging and Deployment are done on the same resource. Now, the namespaces can be according to the stage of the rollout of the product. Also, the different stages can still refer the same utility services (present under nginx or elastic search namespaces). this also applies for Blue/green deployment.

- **How namespace work and how to use it?**
- Each namespace __MUST__ define its own configmap. Same guideline applied on secret.
- However, __service__ is accessible in another namespace. One needs to refernce the namespace before the parameter value as shown below:

![accessserviceinanothernamespace](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/accessserviceinanothernamespace.png)

**NOTE**:
- There are cerain components whichlive globally within in a cluster and hence, these components __cannot__ be created within a namespace. They are: __volume__ and __node__.

The following command lists components that are not bound by a namespace.

~~~
kubectl api-resources --namespaced=false
~~~




