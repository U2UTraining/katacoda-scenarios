In this step, you will use a service to expose your deployment.

Currently you are using a proxy to communicate with your pods. This however is not possible for real-world traffic. To organize your traffic, you need a **service**.

You need to expose nodeapp to the external world. That means you only have two options: **NodePort** and **LoadBalancer**. Unlike Azure, Amazon and other cloud providers, minikube doesn't have an implementation for a load balancer, so you'll need to use NodePort.
NodePort allows the external world to access your container as follows &lt;NODE-IP&gt;:&lt;NODE-PORT&gt;. The NODE-PORT is then mapped to the port used by the container. The obvious downside here is that you need to know the NODE-ID and that node needs to be externally accessible.

There is one extra difficulty here. Remember that you now have 3 pods with dynamically assigned names? How is your service going to know which pod to talk to? The answer here is **labels**. Maybe you didn't notice, but there is already a label in your deployment.yaml file.

this part:

<pre>
template:
  metadata:
    labels:
      app: nodeapp
</pre>

That means that all pods created by this deployment, will have the label "nodeapp" assigned to it. You can check that using the following command:

`kubectl get pods -l app=nodeapp`{{execute T1}}

Copy the following content into service.yaml:

<pre class="file"
  data-filename="./service.yaml"
  data-target="replace">
apiVersion: v1
kind: Service
metadata:
  name: nodeapp-svc
  labels:
    app: nodeapp
spec:
  type: NodePort
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30080
  selector:
    app: nodeapp
</pre>

And deploy it using

`kubectl create -f service.yaml`{{execute T1}}

Notice the selector. At runtime, the service will locate all pod machting its selector. Also notice the port mapping. Port 8080 is the one exposed by the container, port 30080 maps to that port (needs to be in 30000-32767 range). That means you can access it using &lt;NODE-IP&gt;:30080.

`curl host01:30080`{{execute T1}}

Execute the command multiple times. You will see responses from different nodes.


Finally, execute the following command:

`kubectl get services`{{execute}}

Notice that there is no External-IP address assigned to the service. Minikube simply doesn't expose it. In a real-world scenario we will need a cloud provider (or on-prem) that can handle those scenarios.
