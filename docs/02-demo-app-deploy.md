# Dev enviroment : App deploy

Demo Application Deployment (Redis + Dashboard)

This guide covers deploying a demo Dashboard application using Redis as the backend storage layer. 
The focus is on Kubernetes manifests, services, and validating connectivity and deployments, not on developing the application itself.

the full guide is here : https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

---

Overview of the Demo Application

Components:
- Redis Leader
- Redis Follower
- Front-end Dashboard

The Front-end  reads from the Redis follower but write on the Redis Leader. the follower acts as replcias of the Leader.

## Steps 

### Check  version  

``` kubectl version  ```

For k8s versions before 1.9.0 use apps/v1beta2  and before 1.8.0 use extensions/v1beta1

2️⃣Deploy and expose 

Apply both Deployment and Service for each component: Redis Leader, Redis Followers, and the Frontend
Note:Leader is only one pod, follower are 2 pods and front-end 3 pods.


```bash
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/deployment-redis-leader.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/deployment-frontend.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/deployment-redis-follower.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-redis-leader.yaml
kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-redis-follower.yaml

kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-frontend.yaml # This Service uses type nodeport 

kubectl apply -f https://github.com/tkeijock/k8s-eks-showcase/raw/main/k8s/service-frontend-eks.yaml # This Service uses type LoadBalancer**
```
The default Redis Services are only accessible within the Kubernetes cluster because the default Service type is ClusterIP.

To allow users to access the frontend from outside the cluster, the Service must be made externally visible. There are a few options:
-    ```kubectl port-forward``` for temporary access (it keep using ClusterIP)
- Change to  NodePort Service.
  
### **Service of type LoadBalancer**

In a production cloud environment, it is preferable to use a Service of type LoadBalancer if supported by the cloud provider. This Service type uses a Kubernetes component that communicates with the cloud provider's API to provision an external load balancer.
Typically, this is implemented using a Kubernetes Ingress controller and Ingress resources to centralize incoming traffic to the cluster.

