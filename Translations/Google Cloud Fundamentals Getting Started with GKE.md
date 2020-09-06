# Google Cloud Fundamentals: Getting Started with GKE
## Objectives:
In this lab, you will learn how to perform the following tasks:
  - Provision a Kubernetes cluster using Kubernetes Engine.
  - Deploy and manage Docker containers using kubectl.
## *Steps*

1. Start a Kubernetes Engine cluster
 - In GCP console, on the top right toolbar, click the Open Cloud Shell button and click continue.
 - For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:
```
export MY_ZONE=us-central1-a
``` 
  - Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:
```
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
``` 
*It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you; so, be patient.*
 - After the cluster is created, check your installed version of Kubernetes using the kubectl version command:
```
kubectl version
```
 - To get cluster credentials 
```
gcloud container clusters get-credentials webfrontend
```

3. Run and deploy a container
 - From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.) 
```
kubectl create deploy nginx --image=nginx:1.17.10
```
*Note: In Kubernetes, all containers run in pods*
 - To View the pod running the nginx container: 
```
kubectl get pods
```
 - To Expose the nginx container to the Internet:
```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```
 -To view the service on which the deployment is exposed:
```
kubectl get services
```
*It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated.*
 -Use the displayed external IP address to test and contact the nginx container remotely. Open a new web browser tab and paste your cluster's external IP address into the address bar. 
*The default home page of the Nginx browser is displayed.*
 -Scale up the number of pods running on your service:
```
kubectl scale deployment nginx --replicas 3
```
 -To Confirm that Kubernetes has updated the number of pods:
```
kubectl get pods
```
 -Confirm that your external IP address has not changed:
```
kubectl get services
```
 -Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.

### This is the end of the lab
