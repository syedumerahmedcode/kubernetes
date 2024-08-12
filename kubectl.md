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
kubectl create deployment mongo-depl --image=mongo


Get the current status of deployment.

~~~
kubectl get deployment
~~~

Get the replica set present in the cluster

~~~
kubectl get replicaset
~~~

![layersofabsraction](https://github.com/syedumerahmedcode/kubernetes/blob/master/images/layersofabsraction.png)
NOTE: All the CRUD operations (Create, Update, Delete and Read) happens at the deploament level and everything underneath is taken care of automatically.



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


This command is used to describe the pod using kubectl.

~~~
kubectl describe pod [pod name]
~~~


This command starts an interactive terminal for a pod using kubectl. Using this command, one goes to the terminal of the application(running inside the pod).

~~~
kubectl exec -it [pod name] -- bin/bash
~~~


The following command is used to delete a deployment using kubectl.

~~~
kubectl delete deployment [name]
~~~











Template:

command description goes here

~~~
kubectl goes here
~~~
