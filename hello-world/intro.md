# U2U Kubernetes Lab

In this lab you will set up your first Kubernetes cluster. You will learn about 
- Creating and Deploying Pods
- Using kubectl and YAML files
- Setting up communication with Services
- Using Deployments for Scaling
- Updating versions of your app

## Before you start
The online terminal is a pre-configured Linux environment that can be used as a regular console (you can type commands). Clicking on the blocks of code followed by the ENTER sign will execute that command in the terminal.

You will use a sample python aplication calles azure-voting-app-redis (TODO replace with our own?). The name might be mislaeading because right now. it has nothing to do with azure.
You can find the code here [here](https://github.com/Azure-Samples/azure-voting-app-redis)
you can find the docker image [here](https://hub.docker.com/r/microsoft/azure-vote-front-redis/)
The application consists out of two containers
- a backend using redis
- a front-end using python 
