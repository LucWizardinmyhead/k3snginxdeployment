Deploying and managing Nginx Service on K3s via terminal

Author: Lucas Jones

This week, I want to challenge myself by interacting directly with my local K3s cluster through the terminal.

In this post, I’ll walk through deploying a Nginx web server, exposing it, scaling the pods, rolling out an update, viewing logs, and cleaning it up.

Tools Used:

    Linux distro

    K3s installed locally

    kubectl

    ip (to check machine IP)

    curl (to test services)

How to deploy Nginx web server? 

First, I created a simple deployment running the official Nginx container image:

    kubectl create deployment web-server --image=nginx

I verified the deployment and pods:

    kubectl get deployments
    kubectl get pods

Exposing the Deployment as a Service

To make the Nginx server accessible, I exposed it as a Node Port service

    kubectl expose deployment web-server --type=NodePort --port=80

I then checked the service details:

    kubectl get services

The output listed the assigned NodePort (a port in the 30000-32767 range) I used to access the service from my machine’s IP.

 Finding the Machine’s IP

To test the service externally, I retrieved my machine’s IP address:

    ip addr

With the NodePort and IP address, I tested the Nginx service using curl:

    curl http://<machine-ip>:<node-port>

Next, I scaled the number of running Nginx pods to 3:

    kubectl scale deployment web-server --replicas=3

I confirmed the scaling:

    kubectl get pods -o wide

Three pods appeared, all managed by the same deployment.

To view logs for one of the running pods:

    kubectl get pods
    kubectl logs <pod-name>

This gives insight into Nginx’s access logs and container activity.

I updated the Nginx image to a newer version using:

    kubectl set image deployment/web-server nginx=nginx:1.25-alpine

And monitored the rollout process:

    kubectl rollout status deployment/web-server

Destroying the web server:

    kubectl delete service web-server
    kubectl delete deployment web-server

This workflow gave me a deeper understanding of Kubernetes fundamentals.
