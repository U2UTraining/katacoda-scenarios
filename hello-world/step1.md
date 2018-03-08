This is your first step.

## Task

Start Kubernetes with the following command

`launch.sh`{{execute}}

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
