In this step, you will replace your pod with a deployment. This is a far more flexible option that allows for easier scaling and upgrading.

Copy the following content into deployment.yaml:

<pre class="file"
  data-filename="./deployment.yaml"
  data-target="replace">
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nodeapp
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nodeapp
    spec:
      containers:
      - name: nodeapp
        image: u2utraining/nodejs-http-server:v1
        ports:
        - containerPort: 8080
</pre>

And deploy it using

`kubectl create -f deployment.yaml`{{execute T1}}

As you can see this file is very similar to the pod.yaml file you made in the previous step. The kind was set to **Deployment**, and some new properties (like replicas). 
Also notice that no name property is set in the template's metadata. The name is chosen dynamically when deploying the deployment.

Use 

`kubectl get deployments`{{execute T1}}

to verify the state.

- The DESIRED state is showing the configured number of replicas
- The CURRENT state show how many replicas are running now
- The UP-TO-DATE is the number of replicas that were updated to match the desired (configured) state
- The AVAILABLE state shows how many replicas are actually AVAILABLE to the users

Use

`kubectl get pods -o wide`{{execute T1}}

to see your pods. Notice the name that was given to it.

Let's use your proxy to verify that it's running properly. First place the pod name into a variable:

`export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')`{{execute T1}}

Check the value:

`echo Name of the Pod: $POD_NAME`{{execute T1}}

And then do the same request as in the previous step.

`curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/`{{execute T1}}

That should be working.

Let's scale up the application. Update description.yaml to use 3 replicas:

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
        image: landerdocker/nodejs-http-server:v1
        ports:
        - containerPort: 8080
</pre>

To update the deployment, you can not use the **create** command. The deployment already exists! But you can use the **apply** command to update the deployment.

`kubectl apply -f deployment.yaml`{{execute T1}}

Use

`kubectl get pods -o wide --watch`{{execute T1}}

To see the incoming Pods. Use `Ctrl+C` to stop the command. Once again you can use the curl command with the various pod names to see if they are working. Replace $POD_NAME with the pods names and don't forget the `/` at the end.

`curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/`{{execute T1}}

