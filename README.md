# Kubernetes

## Video source:
https://www.youtube.com/watch?v=X48VuDVv0do&ab_channel=TechWorldwithNana

Timestamp: 22:30 

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


