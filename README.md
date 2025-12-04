# k8s-eks-showcase : Overview 

This repository documents my learning journey exploring Kubernetes, specifically on AWS EKS (Elastic Kubernetes Service). It serves as a permanent reference for the steps, concepts, and commands needed to reproduce the setup.

The goal is not just to store notes, but to provide a structured guide that allows me to revisit configurations easily, experiment locally in a test environment, and understand how Kubernetes behaves in AWS in terms of scalability, availability, and integration with cloud services.

All configurations are first validated in a lightweight local cluster using Minikube before being deployed to EKS, ensuring a smooth development-to-production workflow.

# 🏗️ Architecture Overview

## EKS overview

When working with Kubernetes, we rely on a cluster to provide the underlying infrastructure required to run and manage containerized applications.
In a traditional on-premises setup, provisioning and maintaining this infrastructure manually is time-consuming and operationally heavy. 

AWS EKS removes that burden by providing a fully managed Kubernetes control plane, while EC2 instances serve as scalable and highly available worker nodes that I manage directly through kubectl commands.

Although the main goal of this repository is to deploy a real application to EKS, every configuration is first validated in a lightweight local Kubernetes cluster using Minikube.
This approach allows me to test manifests, networking, service exposure, and component interactions before moving to a production-grade environment in AWS.

## Development → Production Flow

To avoid deploying untested configurations directly into EKS, I use Minikube as my local development environment.
Minikube runs a complete Kubernetes cluster inside a single virtual machine, combining the control plane and worker node into one environment. This makes it ideal for experimentation, debugging, and early validation of deployments and services.

In this project, Minikube runs inside a single EC2 instance. Since the instance itself is already virtualized, no additional hypervisor (such as VirtualBox) is needed.
Using Minikube prevents the overhead of provisioning multiple EC2 instances to manually configure them as Kubernetes nodes to form a cluster — an effort that only becomes necessary when progressing to a real EKS deployment.

## Demo Application Overview

The demo application is a simple Dashboard that uses Redis as its backend storage layer. 
This application is interesting because it naturally creates multiple components — a Redis Master, a Redis Slave, and a Front-end — giving us a realistic multi-container scenario inside Kubernetes.

This structure allows us to experiment with deployments, services, and inter-component communication in a way that resembles a real production environment. The goal isn’t to build or improve the application itself, but to use it as a lightweight, practical example while I focus on Kubernetes deployments, service exposure, port and connectivity testing, and validating the full development-to-production workflow.

This structure allows us to experiment with deployments, services, and inter-component communication in a way that resembles a real production environment.  
The goal isn’t to build or improve the application itself, but to use it as a lightweight, practical example while I focus on Kubernetes deployments, service exposure, port and connectivity testing, and validating the full development-to-production workflow.

---
This repository is organized into separate guides to keep the workflow clear and structured:

- **Environment Setup (EC2, Docker, Kubectl, Minikube)**  
  👉 [docs/01-environment-setup.md](docs/01-environment-setup.md)
- **Demo Application Deployment (Redis + Dashboard)**  
  👉 [docs/02-demo-app-deploy.md](docs/02-demo-app-deploy.md)




