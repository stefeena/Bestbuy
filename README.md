# Full-Stack Cloud-Native Development  
## Lab Project Assignment #2: Building a Cloud-Native App for Best Buy



## Table of Contents  

1. [Scenario](#scenario)  
2. [Assignment Objectives](#assignment-objectives)  
3. [Application Overview](#application-overview)  
   - [Services and Descriptions](#services-and-descriptions)  
4. [Project Requirements](#project-requirements)  
   - [Application Architecture](#application-architecture)  
   - [Microservices Development](#microservices-development)  
   - [AI Integration](#ai-integration)  
   - [Kubernetes Deployment](#kubernetes-deployment)  
5. [Assignment Deliverables](#assignment-deliverables)  
   - [GitHub Repository](#github-repository)  
     - [Updated Application Architecture](#updated-application-architecture)  
     - [Application and Architecture Explanation](#application-and-architecture-explanation)  
     - [Deployment Instructions](#deployment-instructions)  
     - [Table of Microservice Repositories](#table-of-microservice-repositories)  
     - [Table of Docker Images](#table-of-docker-images)  
     - [Deployment Files Subfolder](#deployment-files-subfolder)  
   - [Demo Video](#demo-video)  
6. [Resources](#resources)  
7. [Bonus Task: Implement a CI/CD Pipeline](#bonus-task-implement-a-cicd-pipeline)  



### Project Overview  

As a newly appointed *Full-Stack Cloud-Native Developer* at Best Buy, a leading electronics retailer, the task is to design, build, and deploy a demo *cloud-native application* for Best Buy's online store. The application architecture draws inspiration from the *Algonquin Pet Store (On Steroids)* design, with a notable modification: the *Order Queue Service* will utilize a managed backing service such as Azure Service Bus instead of an owned solution like RabbitMQ. The project will leverage a Kubernetes cluster for deployment and aims to showcase the functionality and scalability of a cloud-native application in a retail environment.  

## *Assignment Objectives*  
- Implement a cloud-native application using microservices architecture.   
- Develop and deploy a full-stack solution for Best Buy using Kubernetes.  
- Enable AI-powered product descriptions and image generation using GPT-4 and DALL-E.  
---


## *Application Overview*  


The *Best Buy App* include the following components:  

| Service              | Description                                                                 | Repo                                      |
|----------------------|-----------------------------------------------------------------------------|-------------------------------------------|
| *Store-Front*      | Customer-facing app for browsing and placing orders.                         | [Link](https://github.com/your-repo-link) |
| *Store-Admin*      | Employee-facing app for managing products and viewing orders.                | [Link](https://github.com/your-repo-link) |
| *Order-Service*    | Handles order creation and sends data to the managed order queue.           | [Link](https://github.com/your-repo-link) |
| *Product-Service*  | Handles CRUD operations for product data.                                    | [Link](https://github.com/your-repo-link) |
| *Makeline-Service* | Processes and completes orders by reading from the order queue.             | [Link](https://github.com/your-repo-link) |
| *AI-Service*       | Generates product descriptions and images using GPT-4 and DALL-E models.    | [Link](https://github.com/your-repo-link) |
| *Database*         | MongoDB for persisting order and product data.                              | [Link](https://github.com/your-repo-link) |

---

## References Links to Docker Hub Images

| *Service*         | *Docker Hub Link*                     |  
|---------------------|-----------------------------------------|  
| Store-Front         | [Click Here](https://dockerhub.com/link1) |  
| Store-Admin         | [Click Here](https://dockerhub.com/link2) |  
| Order-Service       | [Click Here](https://dockerhub.com/link3) |  
| Product-Service     | [Click Here](https://dockerhub.com/link4) |  
| AI-Service          | [Click Here](https://dockerhub.com/link5) |  
| Makeline-Service    | [Click Here](https://dockerhub.com/link6) |  



## *Project Requirements*  

### *1. Application Architecture*  
Design the application based on the provided diagram of Algonquin Pet Store(On Steroids).


- ![Arch](images/0arch.png)
- Figure shows the Architecture of Best Buy Website

The architecture you're working with is a cloud-native, microservices-based system designed to support an e-commerce application for Best Buy. This application has multiple services, each responsible for different aspects of the online store's functionality, ranging from product management to order handling, and even AI-driven services. Below is a detailed explanation of each component and its interactions.

### 1. *Store-Front* (Customer-Facing Interface)
The *Store-Front* is the customer-facing application where users can browse products, view details, and place orders. It is the entry point into the system for the customer.

- *Connected to Product-Service: When customers search for or browse products, the Store-Front interacts with the **Product-Service* to fetch product data, including product descriptions, images, and prices.
- *Connected to Order-Service: When customers place an order, the **Store-Front* sends the order details to the *Order-Service* for further processing.

### 2. *Product-Service* (Product Management)
The *Product-Service* is responsible for handling product-related operations. This includes managing product data, such as creating, reading, updating, and deleting product entries (CRUD operations).

- *Linked to Store-Front*: This service is queried by the Store-Front to retrieve product data that is displayed to customers.
- *Linked to Store-Admin: The **Product-Service* also interacts with the *Store-Admin* to allow administrators to manage and update product information.
- *Connected to Store-Admin: The **Store-Admin* (employee-facing interface) allows store administrators to manage the product catalog, update pricing, and perform other administrative tasks related to products.

### 3. *Order-Service* (Order Handling)
The *Order-Service* is responsible for managing order creation, processing, and handling order data flow. This service is crucial for maintaining the overall order lifecycle from creation to completion.

- *Linked to Store-Front: The **Store-Front* sends customer orders to the *Order-Service* once a purchase is made. 
- *Connected to RabbitMQ: Initially, RabbitMQ was used as a messaging system to queue order data for processing asynchronously. However, in your updated architecture, But we also use **Managed Order Queue* (like *Azure Service Bus*), which handles the queuing of orders in a scalable and managed manner.


### 4. *Makeline-Service* (Order Fulfillment)
The *Makeline-Service* processes the orders by reading from the *Managed-Order-Queue*. This service simulates the production line where orders are fulfilled by completing tasks like packaging, inventory management, or preparing the order for shipment.

- *Connected to MongoDB: The **Makeline-Service* is responsible for storing order-related data in a *MongoDB* database. MongoDB is a NoSQL database that stores the order and product data, enabling quick retrieval and scalable storage.

### 5. *Store-Admin* (Admin Interface)
The *Store-Admin* is the internal interface used by employees to manage various aspects of the store, such as products, inventory, and orders. It allows administrators to monitor and update the system's operations.

- *Connected to AI-Service: The **Store-Admin* interacts with the *AI-Service* to enhance the store's functionality with AI-driven features, such as generating product descriptions or images.
- *AI-Service to LLM and DALL-E: The **AI-Service* uses the *LLM (Large Language Model)* like GPT-4 for natural language processing tasks (e.g., product description generation) and *DALL-E* for image generation to create product images or enhancements.


### 6. *AI-Service* (AI-driven Features)
The *AI-Service* provides artificial intelligence capabilities to the system, such as generating product descriptions and creating images.

- *Connected to LLM (GPT-4): The **AI-Service* leverages *GPT-4* to generate descriptive text for products, enhancing the shopping experience by providing rich, dynamic content based on input data.
- *Connected to DALL-E: The **AI-Service* uses *DALL-E*, a generative AI model, to create images of products, which can be used when no product images are available or to create new marketing visuals for items.

### 7. *MongoDB* (Database)
*MongoDB* is used to store the data related to products and orders. As a NoSQL database, MongoDB offers flexibility in handling unstructured data and scaling horizontally.

- *Connected to Makeline-Service: The **Makeline-Service* writes and reads order data from *MongoDB*, enabling efficient processing and persistence of order-related information. This helps in maintaining a record of each transaction, which is crucial for inventory management, shipment tracking, and customer service.

---

### High-Level Workflow

1. *Customer Interaction*:
   - Customers visit the *Store-Front*, browse products, and place orders.
   - The *Store-Front* interacts with the *Product-Service* to show product information.
   - Once an order is placed, the *Store-Front* sends the order details to the *Order-Service*.

2. *Order Processing*:
   - The *Order-Service* queues the order for processing. It initially used RabbitMQ but can use *Managed-Order-Queue* (e.g., Azure Service Bus).
   - The *Makeline-Service* retrieves the order from the queue and processes it.

3. *AI-enhanced Admin Functions*:
   - The *Store-Admin* application allows store employees to manage products, update inventory, and oversee order fulfillment.
   - The *AI-Service* supports the *Store-Admin* by generating product descriptions and images via *GPT-4* and *DALL-E*.

4. *Data Persistence*:
   - *MongoDB* stores the product and order information, ensuring that data is available for quick access and reliable storage.



## *2. Microservices Development*  
You can fork the Algonquin Pet Store repositories and use that as a starting point.

bash
.
├── Store-Front
├── Store-Admin
├── Order-Service
├── Product-Service
├── Makeline-Service
├── AI-Service
└── Database

The tree of the source directories

Thank you for the additional details! Here's an updated version of the technical breakdown based on the technologies you're using:

---

### *1. Store-Front Customization (Vue.js)*

#### *Cloning and Re-purposing*
The *Store-Front* repository was cloned or forked from the original repository to serve as the base for creating a page resembling Best Buy's landing page.

bash
git clone <repo link>


#### *CSS Customization*
The *CSS* files in the *Vue.js* application were edited to match Best Buy’s color scheme and styling guidelines. This includes:

- Background colors
- Button styles
- Text color and fonts

#### *Product Form Modification*
The product form was customized in Vue.js to resemble Best Buy’s product listing interface, which involved:

- Modifying the layout to display product details, descriptions, prices, and images more efficiently.
- Adjusting input fields to fit the Best Buy design.

#### *Logo Update*
The logo in the *Top Navigation* component (created in Vue.js) was replaced with the *Best Buy* logo to ensure consistent branding across the Store-Front.

#### *Best Buy Product Images*
Images of Best Buy products such as desktops, PCs, and iPhones were added to the *src* folder and incorporated into the product listings and relevant sections of the Store-Front.

#### *Local Testing*
To test the *Vue.js* app locally, the following commands were used:

bash
npm install
npm run serve


- ![Arch](images/15UIfromazure.png)
- Figure shows the UI Best Buy Website

This ensures the Store-Front is running properly and the changes are applied.

#### *Docker Build & Push*
Once local testing was successful, the Docker image was built and pushed to Docker Hub for deployment:

To build the Docker image:
bash
docker build -t <image-name> .


To push the Docker image:
bash
docker push <image-name>


---

### *2. Product-Service Updates (Rust)*

#### *File Modification (data.rs)*
The *Product-Service, written in **Rust, was updated by editing the *data.rs** file. The following changes were made:

- *Updated Image Paths*: The image paths for Best Buy products were added or modified to reflect the new product images.
- *Updated Descriptions*: The product descriptions were updated to match Best Buy’s offerings.
- *Price Modifications*: The prices for each product were updated to reflect the correct values for Best Buy's catalog.

The *data.rs* file is crucial in the *Product-Service* as it contains the hard-coded data or links to external resources (like product images, descriptions, and prices). Editing this file ensures the front-end receives accurate and updated product details.

#### *Local Initialization*
After making changes to *data.rs, the **Rust* service was initialized locally by running:

bash
cargo run


Additionally, *pkg-config* was installed to resolve dependencies during the build process.

#### *Connectivity Test*
The *Product-Service* was tested for connectivity with the *Store-Front* to ensure the front-end could access the updated product data through the API.

---

### *3. Order-Service Updates (Node.js)*

#### *Service Initialization*
The *Order-Service, written in **Node.js, was run locally to verify it was functioning properly and could communicate with other services like the **Store-Front* and *Product-Service*.

#### *Microservice Connectivity*
The *Order-Service* was tested for proper integration with the *Store-Front* and *Product-Service. A test order was placed through the **Store-Front, ensuring that the **Order-Service* could process and manage it correctly.

---

### *4. Store-Admin Updates (Vue.js)*

#### *Logo Update*
The *Store-Admin* interface, also written in *Vue.js, was updated by replacing the logo in the **public* folder with the *Best Buy* logo to maintain consistency.

#### *CSS Styles*
The CSS styles for the *Store-Admin* interface were updated to reflect Best Buy’s design, which involved adjusting colors, fonts, and layout components.

#### *Local Testing*
The changes were verified locally by running the *Store-Admin* app and ensuring the logo and new styles were applied correctly. Testing was performed using the following:

bash
npm install
npm run serve


#### *Docker Build & Push*
After successful local verification, the *Store-Admin* app was built and pushed to Docker Hub using the following commands:

bash
docker build -t <image-name> .
docker push <image-name>


---

### *5. AI-Service and Makeline Updates*

1. *AI-Service (No major changes needed)*:
    - The *AI-Service* was minimally adjusted to fit the overall system requirements.
    - The service was verified to work with the other components.
    - Docker images were built and pushed as follows:
    
    bash
    docker build -t <image-name> .
    docker push <image-name>
    

2. *Makeline-Service (Go)*:
    - The *Makeline-Service* was written in *Go* and was tested for connectivity with the other services.
    - After verifying the functionality, the Docker image was built and pushed.

---

### *6. Final Steps*

After all changes were made, tested, and pushed to Docker Hub, the entire system was ready for the next deployment phase. The services were now correctly integrated and aligned with Best Buy’s functionality and branding:

- The *Store-Front* (Vue.js) is connected to the *Product-Service* (Rust) for product data.
- The *Order-Service* (Node.js) handles orders and processes them correctly.
- The *Store-Admin* (Vue.js) allows employees to manage products and view orders.
- The *AI-Service* generates product descriptions and images using GPT-4 and DALL-E.
- All services were Dockerized and pushed for deployment.

This complete set of changes and adjustments ensures the system is aligned with Best Buy’s operations and is ready for production deployment.



# *3. AI Integration*  
- Enable AI-generated product descriptions and images using GPT-4 and DALL-E models.  
- Use Azure OpenAI Services or OpenAI APIs for this integration.  


### *Azure OpenAI Integration with GPT-4 and DALL-E: Technical Documentation*

This documentation provides detailed guidance on integrating *Azure OpenAI* services with *GPT-4* and *DALL-E*. It covers the process of deploying the OpenAI service on Azure, retrieving API keys and endpoints, handling deployment issues that arise when deploying multiple times within a single day, and verifying the deployment status.

---

### *1. Prerequisites*
Before proceeding with the setup, ensure the following:

- *Azure Subscription*: Ensure you have an active Azure subscription and are logged in.
- *Azure CLI*: Install the Azure CLI tool to interact with Azure services.
- *Azure OpenAI Resource*: The OpenAI resource must be created and properly configured (e.g., 0013openai).
  
---

### *2. Create Azure OpenAI Resource*

1. *Navigate to Azure Portal*:
   - Go to the Azure portal and search for "OpenAI" in the marketplace.
   - Select *Azure OpenAI* and create a new instance by specifying the region, subscription, and resource group.

2. *Select Region*:
   - Choose the appropriate region for your service. For this documentation, we will use East US (eastus), but it can be adjusted based on your needs.

3. *Subscription and Resource Group*:
   - Select your subscription (e.g., a110f4f4-bb11-4532-b1f2-cd0e17e3dec6) and resource group (bestbuyaks).
   - Name the resource, e.g., 0013openai.

4. *Deploy Service*:
   - After configuring the OpenAI instance, click *Create* to deploy the service. This might take a few minutes.

---

### *3. API Keys and Endpoints*

Once the Azure OpenAI resource is deployed successfully, you can access the *API key* and *endpoints* needed for connecting to *GPT-4* and *DALL-E* models.

#### *Accessing API Key and Endpoint:*
- *API Key: The API key can be found in the resource settings under the **Keys and Endpoint* section.
- *Endpoint*: The service will provide a base URL (e.g., https://0013openai.openai.azure.com/) for accessing the APIs.

You can retrieve the details by clicking on the *JSON* configuration in your Azure OpenAI resource. For example:

json
{
  "id": "/subscriptions/a110f4f4-bb11-4532-b1f2-cd0e17e3dec6/resourceGroups/bestbuyaks/providers/Microsoft.CognitiveServices/accounts/0013openai",
  "name": "0013openai",
  "endpoint": "https://0013openai.openai.azure.com/",
  "key": "FEPMYPqnRJZEoeCjsN6DFABYgV2m5Cnl25ZdWrRki8edpx151KojJQQJ99ALACYeBjFXJ3w3AAABACOGVkKg",
  "location": "eastus"
}


Copy and securely store the *API Key, **Endpoint, and **Resource Name* (0013openai), as you will use these for subsequent configurations.

---

### *4. Deployment Considerations: Multiple Deployments in One Day*

#### *Deployment Issues with Multiple Deployments:*

- *Problem: Deploying the same Azure OpenAI service multiple times in a single day can cause issues with over-quotas. If you are experiencing such issues, make sure to check the **deployment frequency* and *quota limits* in the Azure portal.
- *Solution*: Azure imposes certain limits on the number of deployments and resources you can create within a given period. To avoid hitting these limits, it is recommended to:
  - Wait for a few hours before attempting additional deployments.
  - Monitor the deployment status to ensure it has completed successfully before deploying another service.

#### *Verifying Deployment:*
To verify that your Azure OpenAI service has been successfully deployed:

1. In the Azure Portal, navigate to the *OpenAI* resource (e.g., 0013openai).
2. Check the *Provisioning State* to ensure it is marked as *Succeeded*.
3. Test the endpoints by using a tool like *Postman* or *curl* to make sure the API key and endpoint are functioning correctly.

Example curl command:

bash
curl -X POST https://0013openai.openai.azure.com/openai/v1/completions \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer FEPMYPqnRJZEoeCjsN6DFABYgV2m5Cnl25ZdWrRki8edpx151KojJQQJ99ALACYeBjFXJ3w3AAABACOGVkKg" \
    -d '{
          "model": "gpt-4",
          "prompt": "Generate a creative product description for an iPhone.",
          "max_tokens": 50
        }'


---


# *4. Kubernetes Deployment*  
- Use Kubernetes to deploy all services.  
- Create ConfigMaps and Secrets for managing application configurations and sensitive information.  

### *Configuring Azure Kubernetes Service (AKS) for GPT-4 and DALL-E Integration*

This guide outlines the detailed steps to configure *Azure Kubernetes Service (AKS)* in the *Azure Portal* for deploying GPT-4 and DALL-E models. The process involves setting up a Kubernetes cluster, configuring agent pools, and preparing the environment for scaling.

---

### *1. Navigate to the Azure Portal*

- Open the [Azure Portal](https://portal.azure.com/).
- Ensure you are logged into your Azure account with appropriate permissions to create resources.

---

### *2. Create a New Azure Kubernetes Service (AKS) Cluster*

1. *Search for AKS*:
   - In the *Azure Portal, search for **Kubernetes Service* or navigate to *Create a resource > Containers > Azure Kubernetes Service (AKS)*.

2. *Choose Resource Group and Region*:
   - Select the appropriate *Resource Group* where the AKS cluster will be deployed. For example, use the resource group *bestbuyaks*.
   - Choose the *Region* as *East US* (or any preferred region that fits your requirements).

3. *Cluster Name*:
   - Enter a *Cluster Name* for the AKS cluster (e.g., bestbuy-aks-cluster).

4. *Availability Zones*:
   - Choose *Availability Zone* as *None* (if this is not a requirement for your deployment).

5. *Pricing Tier*:
   - Select the *Free* pricing tier for cost-effective AKS deployments.

6. *Automatic Upgrades*:
   - *Disable Automatic Upgrades* for the AKS cluster as this may interfere with custom configurations.

7. *Node Security Channel Type*:
   - Set the *Node Security Channel Type* to *No Schedule*. This ensures that no specific scheduling is applied to the nodes during deployment.

8. *Additional Settings*:
   - Leave the remaining settings at their defaults unless specific customization is required.

9. *Click on "Next" and Proceed*:
   - Click *Review + Create* to proceed with the AKS cluster creation.

---

### *3. Configure Agent Pools*

After creating the AKS cluster, configure the *agent pools* for deploying workloads such as GPT-4 and DALL-E models.

#### *Step 1: Update System Pool*

1. *Navigate to the Agent Pools*:
   - After the AKS cluster is created, go to the *Agent Pools* section.
   
2. *Update Agent Pool to System Pool*:
   - *Pool Type: Set the **agent pool* to *System Pool*.
   - *Worker Nodes: Set the **number of worker nodes* to *0* (none).
   - *VM Size: Select **Standard D2 v4* as the VM size for the system pool.

#### *Step 2: Add Worker Pool*

1. *Create Worker Pool*:
   - Add a new *worker pool* with the name worker-pool-std-d2v4.
   - *VM Size: Set the **VM size* to *Standard D2 v4*.
   - *Manual Scaling: Choose **Manual Scaling*.
   - Set the *number of nodes* to *2* for redundancy and high availability.
   
2. *Operating System*:
   - Set both *Operating Systems* for the nodes to *Ubuntu Linux*.

3. *Maximum Pods*:
   - Set the *Maximum Pods* to the default value of *30 pods* per node. This ensures each node can handle up to 30 pods on average.

#### *Step 3: Review Configuration*

- Review the configuration for your agent pools and ensure that the settings match the desired configuration for scaling, VM size, and operating system.

#### *Step 4: Click on "Validate + Create"*

- After reviewing your configurations, click *Validate + Create* to deploy the AKS cluster and agent pools.

---

### *4. Final Configuration and Deployment*

After configuring the AKS cluster, the next step involves validating the deployment to ensure that all resources are properly provisioned.

1. *Validation*:
   - The system will perform a validation check to ensure the configuration meets Azure’s deployment requirements. If there are no errors, you will proceed with the deployment.

2. *Deployment*:
   - Once validated, click *Create* to deploy the AKS cluster with the agent pools.

---

# *5. Configure YML file for project deployment*

After successfully deploying the AKS cluster, you will be ready to deploy all the micro service or business logic plus *GPT-4* and *DALL-E* services in the Kubernetes environment. These models will be containerized and deployed across the agent pools created in this AKS configuration.


Here’s the updated configuration without the ports in the aps-all-in-one.yml file:

---

### *1. Docker Image Configuration in aps-all-in-one.yml (No Ports)*

In the aps-all-in-one.yml file, the Docker images are replaced with the respective custom Docker images uploaded to your Docker Hub. The *ports* configuration has been removed.

yaml
containers:
  - name: product-service
    image: thoufeekx/product-service-l8:latest # Custom product service image

  - name: store-front
    image: thoufeekx/store-front-l8:latest # Custom store front image

  - name: store-admin
    image: thoufeekx/store-admin-l8:latest # Custom store admin image

  - name: order-service
    image: thoufeekx/order-service-l8:latest # Custom order service image

  - name: makeline-service
    image: thoufeekx/makeline-service-l8:latest # Custom makeline service image

  - name: ai-service
    image: thoufeekx/ai-service-l8:latest # Custom AI service image
    env:
      - name: USE_AZURE_OPENAI # Set to True for Azure OpenAI, False for Public OpenAI
        value: "True"
      - name: AZURE_OPENAI_API_VERSION
        value: "2024-07-01-preview"
      - name: AZURE_OPENAI_DEPLOYMENT_NAME # Required if using Azure OpenAI
        value: "gpt-4-deployment" # Replace with actual deployment name
      - name: AZURE_OPENAI_ENDPOINT # Required if using Azure OpenAI
        value: "https://0013openai.openai.azure.com/" # Replace with actual endpoint
      - name: AZURE_OPENAI_DALLE_ENDPOINT
        value: "https://0013openai.openai.azure.com/" # Replace with actual DALL-E endpoint
      - name: AZURE_OPENAI_DALLE_DEPLOYMENT_NAME
        value: "dalle-3-deployment" # Replace with actual DALL-E deployment name
      - name: OPENAI_API_KEY # Always required
        valueFrom:
          secretKeyRef:
            name: openai-api-secret
            key: OPENAI_API_KEY # Reference to the secret for the API Key


---

### *2. Base64 Encoding the API Key*

Before proceeding with the deployment, you need to *Base64 encode* the API key to securely store it in Kubernetes secrets. Here’s how to encode your API key:

1. Open your terminal or command line.
2. Run the following command to encode your API key:

bash
echo -n "<your-api-key>" | base64


3. Replace <your-api-key> with the actual API key from Azure OpenAI.
4. Copy the output, as you will use it to update your Kubernetes secret configuration.

---

### *3. Modify secrets.yaml for API Key*

You need to modify the secrets.yaml file to securely store the Base64-encoded API key. The secret will be used in the aps-all-in-one.yaml file to reference the OpenAI API key.

#### *Example secrets.yaml Configuration*:

yaml
apiVersion: v1
kind: Secret
metadata:
  name: openai-api-secret
type: Opaque
data:
  OPENAI_API_KEY: "<Base64-encoded-api-key>"


1. Replace <Base64-encoded-api-key> with the Base64 encoded API key obtained in the previous step.
2. Save the file and apply it to your Kubernetes cluster using:

bash
kubectl apply -f secrets.yaml


This ensures that the API key is stored securely within Kubernetes.

---

### *4. Modify the aps-all-in-one.yaml Deployment Configuration*

Now, update the aps-all-in-one.yaml file to ensure that all the services are properly configured, including the *AI Service*.

#### *Example Configuration for AI Service*:

Make sure you have the following configuration under the ai-service container in your aps-all-in-one.yaml:

yaml
containers:
  - name: ai-service
    image: thoufeekx/ai-service-l8:latest # Custom AI service image
    env:
      - name: USE_AZURE_OPENAI
        value: "True"
      - name: AZURE_OPENAI_API_VERSION
        value: "2024-07-01-preview"
      - name: AZURE_OPENAI_DEPLOYMENT_NAME
        value: "gpt-4-deployment" # Replace with actual deployment name
      - name: AZURE_OPENAI_ENDPOINT
        value: "https://0013openai.openai.azure.com/" # Replace with actual endpoint
      - name: AZURE_OPENAI_DALLE_ENDPOINT
        value: "https://0013openai.openai.azure.com/" # Replace with actual DALL-E endpoint
      - name: AZURE_OPENAI_DALLE_DEPLOYMENT_NAME
        value: "dalle-3-deployment" # Replace with actual DALL-E deployment name
      - name: OPENAI_API_KEY
        valueFrom:
          secretKeyRef:
            name: openai-api-secret
            key: OPENAI_API_KEY # Reference to the secret for the API Key


This section of the YAML file configures the *AI Service* to use *Azure OpenAI* with the *GPT-4* and *DALL-E 3* endpoints.

---

### *5. Final Deployment Steps (continued)*

After modifying the aps-all-in-one.yaml and secrets.yaml files, follow these steps:

#### *Step 4: Deploy the ConfigMaps and Secrets*

Before applying the deployment YAML, ensure that the necessary *ConfigMaps* and *Secrets* are created and applied properly. 

1. *Deploy the ConfigMap for RabbitMQ Plugins*:
   The config-maps.yaml file may contain configuration data for services such as RabbitMQ. Apply the ConfigMap:

   bash
   kubectl apply -f config-maps.yaml
   

2. *Create and Deploy the Secret for OpenAI API*:
   In the secrets.yaml file, ensure that you have replaced Base64-encoded-API-KEY with your actual *Base64-encoded OpenAI API key*.

   To apply the secret configuration, run:

   bash
   kubectl apply -f secrets.yaml
   

3. *Verify ConfigMaps and Secrets*:
   After applying the ConfigMaps and Secrets, verify their creation by checking the resources in your cluster:

   bash
   kubectl get configmaps
   kubectl get secrets
   

   Ensure that the necessary *ConfigMaps* (e.g., for RabbitMQ) and *Secrets* (e.g., for the OpenAI API key) are listed.

---

#### *Step 5: Apply the Deployment*

Once the secrets and ConfigMaps have been deployed, it's time to apply the main deployment YAML, which includes your services:

bash
kubectl apply -f aps-all-in-one.yaml


This will deploy all the services (e.g., *Product Service, **AI Service, **Order Service, etc.) in your Kubernetes environment. The **AI Service* will be configured with the Azure OpenAI endpoints as specified.

---

# *7. Final Validation*

After applying the deployment, you can verify that your services are running correctly by checking the status of your pods:

bash
kubectl get pods


This command will list the pods running in your Kubernetes cluster, and you can confirm that all services (including AI-related services) are correctly deployed.

You can also check the status of your services and logs to troubleshoot if any issues arise:

bash
kubectl logs <pod-name>


With these steps, your entire application, including the *AI Service* with Azure OpenAI integration, should be successfully deployed in Kubernetes!

---

# *7. Validatin the deployment*

### *Deployment Validation and Testing for Storefront and Store Admin*

#### *1. Validate the Deployment:*

Once the deployment is complete, it’s crucial to validate that all services and pods are correctly deployed and running. You can do this by checking the status of your pods and services using the following commands:

- *Check Pods*:

bash
kubectl get pods


This will give you the status of all the pods running in your Kubernetes cluster. Here’s an example output:


NAME                                READY   STATUS              RESTARTS   AGE
ai-service-57b4f67598-4gt5z         0/1     ContainerCreating   0          6s
makeline-service-6b79f77cbb-sthpx   0/1     ContainerCreating   0          6s
mongodb-0                           0/1     ContainerCreating   0          7s
order-service-cd4d8d759-v9kn4       0/1     Init:0/1            0          7s
product-service-6fbfccc5db-lg29c    0/1     ContainerCreating   0          6s
rabbitmq-0                          1/1     Running             0          7s
store-admin-75f69c89-wz5wt          0/1     ContainerCreating   0          6s
store-front-76dd845669-lxcng        0/1     ContainerCreating   0          6s


- *Check Services*:

bash
kubectl get services


This will provide you with the service details, including their internal IPs and external IPs (if available). Here’s an example output:


NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)              AGE
ai-service         ClusterIP      10.0.3.161     <none>         5001/TCP             13s
store-admin        LoadBalancer   10.0.92.162    <pending>      80:31728/TCP         13s
store-front        LoadBalancer   10.0.187.215   51.8.219.230   80:31255/TCP         13s


Once you’ve obtained the external IP addresses for the *Store Front* and *Store Admin* services, you can proceed to test the applications.

---

#### *2. Test Frontend Access:*

- *Store Front Access*:

You can access the *Store Front* application using the external IP on port 80. For example, if the external IP is 51.8.219.230, visit:


http://51.8.219.230


This will redirect you to the homepage of the *Store Front* where you can see the deployed page showcasing products. The features, such as adding products to the cart and browsing the product catalog, should be fully functional. You can test these features by interacting with the interface, verifying that products are displayed correctly, and the cart functionality works as expected.

- *Store Admin Access*:

Similarly, access the *Store Admin* app via the external IP for *store-admin*. If the external IP is <pending>, this means the service is still provisioning. Once it’s ready, you can access it by visiting:


http://<external-ip-of-store-admin>


In the *Store Admin* interface, you should be able to:
1. *View Orders: All orders made from the **Store Front* should be displayed here.
2. *Complete Orders: Admin can complete an order and send the details to the **MongoDB* database.
3. *Manage Products*: Admins can create new products, edit existing ones, or delete them from the catalog.
4. *Use AI for Product Descriptions and Images: When adding a new product, the **AI Service* will be used to automatically generate product descriptions and images for the new item. This is especially useful for automating and enhancing product listings.

---

#### *3. Access the Azure Portal for Service and Ingress Validation:*

To get a deeper understanding of the services and their configuration, you can use the Azure Portal:

1. *Go to the Azure Portal*:
   - Navigate to your *Azure Kubernetes Service (AKS)* cluster.
   - On the AKS Cluster page, go to the *Services and Ingress* section.

2. *Filter by Namespace*:
   - Choose the default namespace to filter the services running in that namespace.
   - You should see the services that were created, such as store-front, store-admin, and others, listed in the portal. These services should match what you’ve seen in the kubectl get services command.

By following these steps, you can ensure that all your services are running correctly, and the Kubernetes deployment is functioning as expected.

---

#### *4. Additional Testing and Verification*:

- *Verify Product Services*: You should be able to interact with the product service from the Store Front and check if product details are correctly displayed.
- *Verify Cart Functionality: Ensure that the cart allows adding, removing, and checking out products correctly. Ensure product data is stored in **MongoDB*.
- *Verify AI Integration: Test the **AI Service* by adding a new product via the *Store Admin* page and confirm that the AI-generated description and image are properly assigned to the new product.
- *Monitor Logs*: If there are issues, check the logs for each service:

bash
kubectl logs <pod-name>


This will help you troubleshoot any issues that may arise during deployment or runtime.

---

By following these steps, you should have a fully functional environment where the *Store Front* and *Store Admin* services are correctly deployed and integrated with the necessary AI-powered features.

---



## *Assignment Deliverables*  

Your submission must include the following:

### *1. GitHub Repository*
The GitHub repository must include:  
1. A README.md file containing:  
   - *Updated Application Architecture*:  
     - Draw the updated architecture diagram using Draw.io and include it in the README.  
   - *Application and Architecture Explanation*:  
     - Briefly explain the application functionality and how the architecture works.  
   - *Deployment Instructions*:  
     - Step-by-step instructions to deploy the application in a Kubernetes cluster.  
   - *Table of Microservice Repositories*:  
     - A table listing each microservice repository and its GitHub link.  
     - Example:
       | *Service*         | *Repository Link*                       |
       |---------------------|-------------------------------------------|
       | Store-Front         | <GitHub Link>                           |
       | Order-Service       | <GitHub Link>                           |
   - *Table of Docker Images*:  
     - A table listing all Docker images you created, including their names and links to their Docker Hub repositories.  
     - Example:
       | *Service*         | *Docker Image Link*                     |
       |---------------------|-------------------------------------------|
       | Store-Front         | <Docker Hub Link>                       |
       | Order-Service       | <Docker Hub Link>                       |
    - *Any issues or limitations in the implementation (Optional)*

2. A *Deployment Files* subfolder:  
   - Include all Kubernetes deployment YAML files in a folder named Deployment Files.  
   - Ensure these files are clearly named (e.g., store-front-deployment.yaml, order-service-deployment.yaml).   

### *2. Demo Video*  
Record a *5-minute max demo video* showcasing the following:  
- The application in action after deployment to AKS cluster.  
- AI-generated product descriptions and images.  
- Integration with the managed order queue service.  

*Upload the video to YouTube* and include a link to the video in your README.md file under a "Demo Video" section.  

---

## *Grading Criteria*  

| *Criteria*                           | *Weight* |
|----------------------------------------|------------|
| Application Code and Documentation     | 20%        |
| Integration with Managed Order Queue   | 20%        |
| AI Integration (GPT-4 and DALL-E)      | 20%        |
| Kubernetes Deployment                  | 20%        |
| Demo Video                             | 20%        |

---

## *Resources*  
- *Algonquin Pet Store Repository:* [GitHub Link](https://github.com/ramymohamed10/algonquin-pet-store-on-steroids)  
---

## *Submission Instructions*  
1. Push your code and deployment files to your GitHub repository.  
2. Submit the GitHub repository link via Brightspace.  

--- 

### *Bonus Task: Implement a CI/CD Pipeline for Each Microservice (2 Bonus Marks)*  
Set up a *Continuous Integration/Continuous Deployment (CI/CD) pipeline* for all microservices to automate building, and deploying the application.