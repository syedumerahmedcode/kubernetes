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

The following command takes the configuration file as input parameter and does watever is written there. This command is used in both when a deployment is created or updated as kubectl detects it itself.


For example, kubectl apply -f nginx-deployment.yaml

touch nginx-deployment.yaml
vim nginx-deployment.yaml



The following command outputs the status in a temporary yaml file.

~~~
kubectl get deployment nginx-deployment-o yaml > nginx-deployment-result.yaml
~~~

The above command will output the status in nginx-deployment(the temporary yaml file).


The fllowing command deletes the K8s component using the configuration file.
~~~
kubectl delete -f [file name]
~~~
For example;
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml


This command gets all coponents inside the cluster.

~~~
kubectl get all
~~~




Template:

command description goes here

~~~
kubectl goes here
~~~
