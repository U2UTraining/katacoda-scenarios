## Creating Pods

backend pod:

<pre class="file"
  data-filename="./files/pod-front.yaml"
  data-target="replace">Contents To Copy To Editor</pre>

test:

<pre class="file"
  data-filename="./pod.yaml"
  data-target="replace">
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
</pre>

test:

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