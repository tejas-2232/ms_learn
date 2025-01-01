## Orchestrate containers for cloud-native apps with AKS 

**Introduction**

When you create cloud-native applications, you can leverage the benefits of containers, which allow you to bundle and run applications. Modern software systems are increasingly using containers as part of their architecture. Isolating system components in containers gives developers the flexibility to use the right technologies where they’re needed, while also extending applications without changing existing system architecture. As applications grow to include numerous containers deployed across multiple servers, operating them becomes more complex.

Many cloud-native architectures turn to Kubernetes to deploy and manage containers. Kubernetes, often abbreviated as K8s, provides a framework to scale, load balance, and self-heal applications. If a container goes down, Kubernetes can start another container automatically or even replicate containers when demand increases.

Azure Kubernetes Service (AKS) is a fully managed Kubernetes service that offloads a lot of the complexity, security, and operational overhead.

### Example scenario: Connecting fridges, at scale

Let's say you work for Adatum Corporation, a manufacturer of home appliances. You lead a small development team there, and you've been tasked with building an app for smart fridges.

Cloud-native apps have loosely coupled functionality by nature. With AKS, we can be more agile in our design and don't need to predict future requirements. We can start by using AKS for a small fridge inventory-management app that informs businesses about what needs to be restocked.

We start by using an AKS cluster to deploy a Node.js container, which will process messages from the fridges and send them to a management web app. Later, if needed, we can add functionality to the app, such as connecting to fridge telemetry and onboard sensors.

<hr> 
<hr>

## Running containers with Kubernetes

Containers are a virtualization technology. In many ways, they’re similar to virtual machines (VMs), but containers don't have their own internal operating system (OS). Containers share part of the OS with their hosts. As such, a single VM can run many containers. However, each container is still self contained with its own code, data, and dependencies.

You can build and run containers in Docker. Docker is software for containers that provides a widely used standard for packaging and distributing containerized applications. With Docker, you can also store and share container images.

## Using containers in the cloud

Azure Container Registry provides storage for Docker container images in the cloud, based on the open-source Docker Registry 2.0. Azure Container Registry provides many security benefits, such as:

Authentication for who can see and use your images.

You can sign images to increase trust and reduce the chances of an image becoming accidentally, or intentionally, corrupted.

All images stored in a container registry are encrypted at rest.

Azure Container Registry also lets you automate tasks like container image builds and app redeployments when an image is rebuilt.


## Using Azure Container Registry

In our example scenario, your team needs to host a Docker image in Kubernetes that connects messages from smart fridges to a management web app. We'll create a container registry to store the image, and later, ACR will connect to an AKS cluster for image deployment.

## Manage containers in the cloud

Kubernetes orchestrates containers by managing VMs for you and scheduling containers to run in those VMs based on your resource requirements. If the need arises, you can automatically scale to multiple identical containers
![image](https://github.com/user-attachments/assets/49b5f952-6629-4cb8-baac-965bac14ed48)


## AKS and Kubernetes

AKS handles Kubernetes for you by deploying, managing, and scaling Kubernetes clusters. If you have to replace or replicate a container, AKS automatically routes and balances traffic in the cluster. AKS makes it simple to deploy, manage, and connect containerized applications, providing massive savings in development time, application deployment, and security obligations.

## Creating the smart fridge solution

In our scenario, we'll use AKS to host containers in the cloud. The smart fridges will send REST messages to the cloud, where AKS will receive them and route them to a container. The container will run a Node.js program that routes messages to a management web app.
