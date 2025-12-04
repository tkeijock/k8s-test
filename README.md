# k8s-eks-showcase : Overview 

This repository exists to document my learning journey while exploring Kubernetes , specifically using AWS EKS (Elastic Kubernetes Service). This repository serves as a permanent reference for the essential steps, concepts, and commands required to reproduce the entire setup whenever needed.

The idea is not just to store notes, but to provide a structured guide that enables me to:

- Have a central place to revisit configuration and deployment steps without repeatedly searching for scattered information

- Experiment locally—using a controlled test environment—before applying the same concepts to production workloads on EKS

- Understand how Kubernetes behaves when running inside AWS, especially in terms of scalability, availability, and infrastructure integration


# 🏗️ Architecture Overview

## EKS overview
When working with Kubernetes, we rely on a cluster to provide the underlying infrastructure required to run and manage containerized applications.
In a traditional on-premises setup, I would be responsible for provisioning and maintaining these elements manually. 

AWS removes that operational burden through EKS by provisioning a fully managed Kubernetes control plane and uses EC2 instances as worker nodes to run workloads. By leveraging EC2, these nodes inherit the capabilities of the AWS cloud, including:
- High availability
- Automatic scalability 
- Multi-AZ redundancy 
- Native AWS integrations 

In essence, EKS provides Kubernetes with cloud-grade reliability, where EC2 instances act as worker nodes that I can manage directly through kubectl.

## Development → Production Flow

Before deploying anything to EKS, we first need a running machine with Minikube installed to serve as our development environment.
This environment allows us to test and validate the application locally before promoting it to a production-grade Kubernetes cluster.

## EKS vs Minikube 
Minikube provides a lightweight, local Kubernetes cluster that runs inside a single virtual machine. It does not create multiple virtual machines, nor multiple physical nodes . Instead, it simulates a Kubernetes cluster by running all core components (control plane + worker node) on the same machine (the nodes are logical, not separate machines).

In this test environment, I will use Minikube to simulate a Kubernetes cluster inside a single EC2 instance. Minikube provides a lightweight, local cluster that is perfect for learning and experimentation.

Without Minikube, I would need to provision multiple EC2 instances, configure them as Kubernetes nodes, and manage all the required infrastructure myself — something that only makes sense later, when moving to a real EKS production environment.

### Minikube: Local vs Virtual
When running Minikube on a local machine, a virtualization layer is required—typically something like VirtualBox. However, when using a virtualized environment such as an EC2 instance, an additional hypervisor is not needed, because the instance already provides the underlying virtualization required for Minikube's nodes.

---

# Dev enviroment : App deploy

The demo application is a simple Dashboard that uses Redis as its backend storage layer. This application is interesting because it naturally creates multiple components — a Redis Master, a Redis Slave, and a Front-end — giving us a realistic multi-container scenario inside Kubernetes. This structure allows us to experiment with deployments, services, and inter-component communication in a way that resembles a real production environment. The goal isn’t to build or improve the application itself, but to use it as a lightweight, practical example while I focus on Kubernetes deployments, service exposure, port and connectivity testing, and validating the full development-to-production workflow.




