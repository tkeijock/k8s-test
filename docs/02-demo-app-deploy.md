# Dev enviroment : App deploy

This guide walks through deploying the demo application on a Minikube cluster running inside an EC2 instance.

The application is a simple Dashboard that uses Redis as its backend storage layer. The frontend writes to the Redis Leader and reads from the Redis Follower, which acts as a replicated copy of the Leader.

The goal here is not to develop or improve the application itself, but to practice Kubernetes deployments, services, pod communication, and connectivity validation in a cloud-aligned environment.

This deployment is based on the official Kubernetes Guestbook tutorial, which demonstrates how to run this application in a generic Kubernetes environment.

You can find the original guide here:
https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

In this repository, I take the core concepts from the official guide and adapt them to a cloud-focused development workflow.
That includes:

- using Kubernetes components to reproduce production-like behavior within the development environment
-  running Minikube inside an EC2 instance for controlled in-cluster app testing 
- validating all manifests before deploying them to AWS EKS

---

Overview of the Demo Application

Components:
- Redis Leader
- Redis Follower
- Front-end Dashboard

## Steps 

### 1️⃣ Check  version  

``` kubectl version  ```

if needed change inside the .yaml fies :

- Api version:  Use extensions apps/v1 for >=1.9 , , apps/v1beta2 for 1.8–1.8.x and /v1beta1 for <1.8

- For frontend dns : Use 'dns' for Kubernetes >=1.13 (CoreDNS), or 'env' for <=1.12 (older clusters without DNS)

> Here is set to apps/v1  and 'dns'

### 2️⃣ Deploy and expose 

Apply both Deployment and Service for each component: Redis Leader, Redis Followers, and the Frontend

> Note: redi-Leader is only 1 pod, redis-follower 2 pods and front-end 3 pods.

```bash
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/deployment-redis-leader.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/deployment-frontend.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/deployment-redis-follower.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-redis-leader.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-redis-follower.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-frontend.yaml # This Service uses type nodeport 
```

### Frontend service :

By default, the frontend Service is of type ClusterIP, which means it is only accessible within the Kubernetes cluster.
To allow external access, you can make the Service externally visible in several ways:

- Use kubectl port-forward for temporary access (the Service remains ClusterIP).

- Change the Service type to NodePort (recommended for local clusters or testing).

- Change the Service type to LoadBalancer (for clusters running in cloud environments, e.g., EKS).

> **Frontend Service is configured as NodePort to allow predictable port access from outside the cluster**

### **Service of type LoadBalancer**

In a production cloud environment, it is preferable to use a Service of type LoadBalancer if supported by the cloud provider. This Service type uses a Kubernetes component that communicates with the cloud provider's API to provision an external load balancer.

Typically, this is implemented by exposing an Ingress controller that runs in the cluster. The Ingress controller centralizes incoming traffic and routes it to the appropriate Ingress resources.

## Testing 

1️⃣ Check service port
if the port was not specified in the service, it is possible to check this command to show url and port:

``` minkube service fronted --url ```

service-frontend.yaml in this repository uses :  **PORT= 31080** 

2️⃣ Permit acess in the cloud
 
in the cloud, create a security group with the above Port and associate it to the instance

3️⃣Acess the deployed app:

Check the instance public IP on the aws console and acess it on a broswer:
<ec2-public-ip>:31080
