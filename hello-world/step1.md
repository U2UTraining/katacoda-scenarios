This is your first step.

## Getting Things Up and Running

Start Kubernetes with the following command:
(you can simply click the box)

`launch.sh`{{execute}}

Wait until Kubernetes is done loading. Then use the following command to verify that **kubectl** is installed:

`kubectl version`{{execute}}

You should be able to see both the version of the client(kubectl version) as well as the server(Kubernetes version).

Let's take a look at the cluster itself. The following command should give you some basic information, like where the master is running.

`kubectl cluster-info`{{execute}}

Finally let's see how many nodes we have available:

`kubectl get nodes`{{execute}}

Just one for now.



## Creating Pods

backend pod:

```
apiVersion: v1
kind: Pod
metadata:
    name: redis
    labels:
        name: redis
spec:
  containers:
  - name: azure-vote-back
    image: redis
    ports:
    - containerPort: 6379
      name: redis
```{{copy}}
