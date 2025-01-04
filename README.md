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

## Exercise - Create AKS Cluster

**What is covered in the exercise?**

* Create a new registry in Azure Container Registry using the Azure portal.
* Build an express.js Docker image and upload it to your container registry.
* Create a Kubernetes cluster using AKS and connect it to your container registry.
* Build a Next.js Docker image and upload it to your container registry.


## Create a registry in Azure Container Registry

1. Sign in to the Azure portal with your Azure subscription.
2. Select Create a resource > Containers > Container Registry.

![image](https://github.com/user-attachments/assets/fc552ca5-d7f3-4239-a1be-8452695a6418)

3. Specify the values in the following table for each of the properties:
![image](https://github.com/user-attachments/assets/d2d70173-7bb5-4ef8-bd15-ae34fa2ac07d)

4. Select Review + create > Create.
The container registry takes a few minutes to create.

## Build a Docker image and upload it to Azure Container Registry

1. Navigate to Azure Cloud Shell. If you're prompted to choose a shell, select Bash.
2. Create environment variables for your registry name and resource group using the following commands. Make sure you replace {registry_name} with your unique registry name.

```bash
# Set the registry name
REGISTRYNAME={registry_name}

# Set the resource group name
RESOURCEGROUP=learn-cna-rg
```
3. Download the source code for the Node.js app from GitHub using the git clone command.

```bash
git clone https://github.com/MicrosoftDocs/mslearn-cloud-native-apps-express.git
```
4. Change directories to the source code folder using `cd`.

```bash
cd mslearn-cloud-native-apps-express/src
```

5. Build and store the Docker image in your container registry using the az acr build command. Make sure to include the . at the end of the command.

```bash
az acr build --registry $REGISTRYNAME --image expressimage .
```

6. Return to the main directory of the source code using cd ...

```bash
cd ..
```
#### Note: 5th Step might not work if you are using Azure Free Trail

### Alternate way to Push Image to ACR?

1. login to ACR using docker login command in bash terminal
2. if image is not build already then build the image and then tag it.
3. push the tagged image to ACR.

The commands are as follows:

**Docker login:**

> Use password and username specified in azure portal > Container registry > Settings > Access Keys

```bash
docker login marvelregistry.azurecr.io
```

![image](https://github.com/user-attachments/assets/fdaa654c-afec-48fe-97a9-1f0a34fc71f0)


**Tag Docker image:**
```bash
1. docker tag <the_name_you_want_to_show_in_ACR> <youracrname>.azurecr.io/<the_image_name_which_you_built>
2. docker push <youracrname>.azurecr.io/<the_name_you_want_to_show_in_ACR> 
```

**Actual command I used:**

```bash
1. docker build -t marvelregistry.azurecr.io/expimgv1 .
```
![image](https://github.com/user-attachments/assets/ce7864de-979b-4385-8b55-f366693a23b8)

```bash
2. docker push marvelregistry.azurecr.io/expimgv1
```
![image](https://github.com/user-attachments/assets/10efd096-e0cd-41ff-b7d3-fe2e6dcf64af)

* The Docker file contains the step-by-step instructions for building a Docker image from the source code for the Node.js application. 
* Azure Container Registry runs these steps to build the image, and as each step completes, a message is generated.
* The build process should finish after a couple of minutes.

_Troubleshooting Ref:_

1. [acr_issue](https://learn.microsoft.com/en-us/answers/questions/1530524/how-to-fix-(tasksoperationsnotallowed)-acr-tasks-r)
2. [How to fix (TasksOperationsNotAllowed](https://stackoverflow.com/questions/77982084/how-to-fix-tasksoperationsnotallowed-acr-tasks-requests-for-the-registry-cont)


## Create AKS Cluster

1. On the Azure portal Home page, select Create a resource.

2. Select Containers > Azure Kubernetes Service (AKS).

![image](https://github.com/user-attachments/assets/06ce32db-9cc9-4049-bb27-3512439c9818)

3. On the Basics tab, enter the following information:

![image](https://github.com/user-attachments/assets/72c6b241-a4d7-4cb8-bca4-a140cad925fe)

4. Select Next > Next > Next.

5. On the Integrations tab, select the container registry you created earlier.

6. Select Review + create > Create.
    -> The cluster takes a few minutes to create.
   
7. Return to the Azure Cloud Shell, and create an environment variable for your cluster using the following command. Make sure you replace `{cluster-name}` with your Kubernetes cluster name

```bash
CLUSTERNAME={cluster-name}
```

### Build the management app Docker image

1. In Azure Cloud Shell, change directories into the source code folder for the management app using cd

```bash
cd react/
```

2. Build and store the Docker image in your container registry using the az acr build command. Make sure to include the . at the end of the command.

```bash
az acr build --registry $REGISTRYNAME --image webimage .
```
