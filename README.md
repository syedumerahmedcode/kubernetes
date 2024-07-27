# Kubernetes

## Video source:
https://www.youtube.com/watch?v=X48VuDVv0do&ab_channel=TechWorldwithNana

Timestamp: 12:49

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
- It is an abstraction over the container.
- -  Note: We do not work with the underlying container technology but rather we only interact with the Kubernetes layer.
- Usually, one application runs per pod. 
- Each pod gets its own IP address(_not the container but the pod itself gets an IP address_).
- Please note that pods are ephemeral ion nature i.e. they can die very easily. When that happens, a new pod is created which takes place of the recently deceased pod.
- New IP address on re-creation of a pod.
![pod](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/pod.png)

### Services
- This creates a permenant IP address for the pod.
- The lifecycles of the service and the pod are **not** connected. This means that if the pod dies, the service and its IP address will remain.This means that the endpoint remains the same.
![service](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/service.png)

### Ingress
- Application is made accessible via a browser using ingress.
- External service is a service that opens the communication to external sources. It is a http IP address of the K8s service and the IP address of the said K8s service.
- Ingress removes the IP address of the K8s service and instead creates a user readable(user friendly) web address. 
- After creating an ingress, the user request first goes to ingress and then it goes to the service.
![ingress](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/ingress.png)

### ConfigMaps
- It contains the external configuration to the application. For example, database name. 
- - **Advantage:** Without configmap(and K8s), if database name chnaged, one would have to rebuild the docker image, push it to docker repository and then pull the image. With configmap, all of these extra steps are not needed anymore.
![configmap](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/configmap.png)


### Volumes


### Secrets





### Statefulset


### Deployment





