In this step, you will create your first pod in the cluster. You can use a YAML file to describe your pod.
Copy the following piece of code to the pod.yaml file.

<pre class="file"
  data-filename="./pod.yaml"
  data-target="replace">
apiVersion: v1
kind: Pod
metadata:
  name: nodeapp
spec:
  containers:
  - name: nodeapp
    image: u2utraining/nodejs-http-server:v1
    ports:
    - containerPort: 8080
</pre>

It uses a simple image that receives HTTP requests and sends back a version and the name of the pod it runs on.

To deploy this pod on the cluster, you need to use the **create** command. This might take a few seconds to complete.

`kubectl create -f pod.yaml`{{execute}}

Notice that you didn't have to specify the resource type. Kubectl knows it's a pod by inspecting the **kind** property in the YAML file.

Check that your pod is running by using the **get** command:

`kubectl get pods`{{execute}}

Make sure the pod is running. If not, wait a few seconds and execute the command again. Alternatively, you can use **--watch** parameter to keep monitoring the pods.

`kubectl get pods --watch`{{execute}}

You can stop this command by using `Crtl+C`.

If you want more information about your pod you can use the following command:

`kubectl describe pod/nodeapp`{{execute}}

Here, "nodeapp" is the name of your pod. If you want a description of all pods, you can simply leave out the name. Notice the events at the end of the description. There, you can see how the image was pulled and a container was created.

Let's see if your pod actually does something. You'll need to talk to that pod. By default, a pod can only be addressed from within the cluster, so you'll need to set up a port forwarding mechanism.
Open a second terminal and run the following command:

`kubectl port-forward nodeapp 4000:8080`{{execute T2}}

This will allow you to talk to the pod directly. You can now send a message to your pod using the following URL:

`curl http://localhost:4000`{{execute T1}}

This sends an HTTP GET to port 8080 of your pod. This port is specified in pod.yaml. This is also the port exposed by the container and used in the nodejs application:

**DockerFile**
```docker
FROM node:8-alpine

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY package.json /usr/src/app/
RUN npm install

COPY . /usr/src/app

EXPOSE 8080
CMD [ "npm", "start" ]
```

**server.js**
```javascript
const http = require("http");
const os = require('os');

const server = http.createServer(function (request, response) {
  response.writeHead(200, {
    "Content-Type": "text/plain"
  });

  response.end("V1 request processed by " + os.hostname() + "\n");
}).listen(8080);
console.log('Listening on port 8080');
```

Ok, good job. Don't forget to clean up before moving to the next part:

`kubectl delete pod --all`{{execute}}

Also stop the port-forwarding in Terminal 2 using `Crtl+C`.
