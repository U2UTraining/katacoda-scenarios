## Scaling with Deployments

In this step, you will replace your pod with a deployment. This is a far more flexible option that allows for easier scaling and upgrading.

Copy the following content into deployment.yaml:

<pre class="file"
  data-filename="./deployment.yaml"
  data-target="replace">
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: webapp1
spec:
  replicas: 1
  template:
    metadata:
    spec:
      containers:
      - name: webapp1
        image: katacoda/docker-http-server:latest
        ports:
        - containerPort: 80
</pre>

And deploy it using

`kubectl create -f deployment.yaml`{{execute T1}}

As you can see this file is very similar to the pod.yaml file you made in the previous step. The kind was set to **Deployment**, and some new properties (like replicas). 
Also notice that no name property is set in the template's metadata. The name is chosen dynamically when deploying the deployment.

Use 

`kubectl get deployments`

to verify the state.

- The DESIRED state is showing the configured number of replicas
- The CURRENT state show how many replicas are running now
- The UP-TO-DATE is the number of replicas that were updated to match the desired (configured) state
- The AVAILABLE state shows how many replicas are actually AVAILABLE to the users

Use

`kubectl get pods`

to see the number of pods. Notice the name that was given to it.

Let's use your proxy to verify that it's running properly. First get the pod name:

`export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}') echo Name of the Pod: $POD_NAME`{{execute T1}}

And then do the same request as in the previous step.

`curl http://localhost:8001/api/v1/proxy/namespaces/default/pods/$POD_NAME/`{{execute T1}}

That should be working.

Let's scale up the application. Update description.yaml to use 3 replicas:

<pre class="file"
  data-filename="./deployment.yaml"
  data-target="replace">
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: webapp1
spec:
  replicas: 3
  template:
    metadata:
    spec:
      containers:
      - name: webapp1
        image: katacoda/docker-http-server:latest
        ports:
        - containerPort: 80
</pre>

To update the deployment, you can not use the **create** command. The deployment already exists! But you can use the **apply** command to update the deployment.

`kubectl apply -f deployment.yaml`{{execute T1}}

Use

`kubectl get pods -o wide --watch`{{execute T1}}

To see the incoming Pods. Once again you can use the curl command with the various pod names to see if they are working.

`curl http://localhost:8001/api/v1/proxy/namespaces/default/pods/$POD_NAME/`{{execute T1}}

