In this step, you will use a upgrade the version of the image running in your cluster.

Let's say you have modified some code and pushed a new version to the container registry. The next step is to deply that new image to the cluster. 

This is actually quite easy. All you need to is update the deployment and apply those changes.

To see the current image of your pods you can use the **describe** command

`kubectl describe pods`{{execute T1}}

Ok, let's get busy. Update your deployment.yaml file.

<pre class="file"
  data-filename="./deployment.yaml"
  data-target="replace">
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nodeapp
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nodeapp
    spec:
      containers:
      - name: nodeapp
        image: landerdocker/nodejs-http-server:v2
        ports:
        - containerPort: 8080
</pre>

to apply the changes you can use the following command:

`kubectl apply -f deployment.yaml`{{execute T1}}

To see the rolling update execute

`kubectl get pods`{{execute T1}}

You can see the various states of your pods(Running, Terminating, ContainerCreating, ...). In addition, you can use the **rollout status** command to check upgrade progress.

`kubectl rollout status deployments/nodeapp`{{execute}}

Finally you can double-check by sending a request:

`curl host01:30080`{{execute T1}}

If the upgrade didn't go as planned, you can always roll back using the following command:

`kubectl rollout undo deployments/nodeapp`{{execute}}

