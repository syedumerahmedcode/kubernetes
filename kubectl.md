# kubectl

## Basic commands


Get the status of the nodes.

~~~
kubectl get nodes
~~~

Get the current status of the pods.

~~~
kubectl get pods
~~~


Get the current status of services.

~~~
kubectl get services
~~~


The following command is used for creating various components using kubectl
~~~
kubectl create -h
~~~

For example;

kubectl create deployment nginx-depl --image=nginx


Get the current status of deployment.

~~~
kubectl get deployment
~~~

Get the replica set present in the cluster

~~~
kubectl get replicaset
~~~

![LayersOfAbsraction](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/LayersOfAbstraction.png)

How to edit an existing deployment(in order to: chnage pod name etc.)

~~~
kubectl edit deployment [name]
~~~
 For example: kubectl edit deployment nginx-depl
--> Upon executing the above command, one gets an auto-generated configuration file with default values.


This command is used to check logs in kuberenetes using kubectl.

~~~
kubectl logs [pod name]
~~~















Template:

command description goes here

~~~
kubectl goes here
~~~
