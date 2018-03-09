If the server hasn't started yet, start the Kubernetes cluster with the following command:

`minikube start`{{execute}}

Minikube is a minimalistic Kubernetes implementation, which is great for learning. It is not meant to be used in production.

Wait until Kubernetes is done loading. Then use the following command to verify that **kubectl** is installed:

`kubectl version`{{execute}}

You should be able to see both the version of the client(kubectl version) as well as the server(Kubernetes version).

Let's take a look at the cluster itself. The following command should give you some basic information, like where the master is running.

`kubectl cluster-info`{{execute}}

Finally let's see how many nodes we have available:

`kubectl get nodes`{{execute}}

Just one for now. Notice the name **host01**, you can use this name to talk to this node directly from within the cluster.