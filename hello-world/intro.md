# U2U Kubernetes Lab

In this lab you will set up your first Kubernetes cluster. You will learn about 
- Creating and Deploying Pods
- Using kubectl and YAML files
- Using Deployments for Scaling
- Setting up communication with Services
- Updating versions of your app

## Before you start
The online terminal is a pre-configured Linux environment that can be used as a regular console (you can type commands). Clicking on the blocks of code will execute that command in the terminal.

You will use a sample node application calles landerdocker/nodejs-http-server.
You can find the code here [here](TODO)
you can find the docker image [here](https://hub.docker.com/r/landerdocker/nodejs-http-server/)
The application consists out of a single container which is a node HTTP server, which responds with its version and the machine it's running on.
