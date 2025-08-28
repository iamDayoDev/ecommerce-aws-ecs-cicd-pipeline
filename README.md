# AWS ECS CI/CD E-Commerce Deployment

This project demonstrates how to deploy an **e-commerce website** on **Amazon ECS** using **AWS Developer Tools** including **CodeCommit, CodeBuild, CodeDeploy, and CodePipeline**. The project highlights containerized application deployment with a fully automated CI/CD pipeline, running in a secure custom VPC with load balancing.

---


## Project Overview

This project aims to automate the deployment of a containerized e-commerce application on **AWS ECS**. Using AWS Developer Tools, the pipeline takes code from the repository, builds a Docker image, pushes it to **ECR**, and deploys it seamlessly to ECS. This ensures that updates to the application are deployed automatically with minimal manual intervention.

---

## Architecture

The project uses the following AWS components:

- **Custom VPC** with public and private subnets  
- **Internet Gateway & NAT Gateway** for secure networking  
- **ECS Cluster** to orchestrate containers  
- **Task Definitions** to define container configuration  
- **ECS Service** to maintain and scale running tasks  
- **Application Load Balancer (ALB)** to route traffic  
- **ECR Repository** to store Docker images  
- **CodeCommit, CodeBuild, CodeDeploy, and CodePipeline** for full CI/CD automation  

---

## Project Steps

### Step 1: Custom VPC Setup

I created a **custom VPC** using the “VPC and More” option in the AWS console. This automatically provisioned public and private subnets, an internet gateway, a NAT gateway, and route tables. The setup allowed the load balancer to run in the public subnet while ECS tasks ran securely in the private subnets.

---

### Step 2: Create ECR Repository

Next, I created a **repository in Amazon ECR**. Using the “View Push Commands” option, I built the Docker image locally and pushed it to ECR. This ensured that my application image was stored securely and ready for deployment.

---

### Step 3: ECS Cluster and Task Definition

I then created an **ECS cluster**, which would host my application containers. Following that, I defined a **task definition** that specified which Docker image to use, CPU and memory allocations, networking configuration, and port mappings — essentially creating a blueprint for running the application.

---

### Step 4: ECS Service Creation

From the ECS cluster, I created a **service**. The service ensured that a desired number of tasks were always running, automatically restarting failed tasks to maintain application availability.

---

### Step 5: Verify ECS Deployment

To check that the ECS deployment was working, I copied the **DNS name of the Application Load Balancer** into a browser. The application loaded successfully, confirming that the ECS tasks were running correctly and accessible externally.

---

### Step 6: Automation with CodePipeline and CodeBuild

Next, I moved to the automation phase. I set up **CodePipeline** to orchestrate the workflow and **CodeBuild** to:

- Pull the latest code from the repository  
- Build the Docker image  
- Push the image to **ECR**  

I also edited the environment variables in `buildspec.yml` to ensure the build process had the correct configuration for connecting to ECS and other resources.

---

### Step 7: Deployment with CodeDeploy

In the deployment stage, I configured **CodeDeploy** and added `imagedefinitions.json` to properly map the build artifacts to ECS tasks.  

During initial runs, I faced errors:

- **Build Phase Permission Error:** CodeBuild couldn’t access ECR. Adding the **AmazonEC2ContainerRegistryPowerUser** policy resolved it.  
- **Deploy Phase Error:** CodeDeploy couldn’t find the ECS container. This was fixed by correcting the container name in `buildspec.yml` and adding `REPOSITORY_URI` to the environment variables.  

Once these issues were resolved, the pipeline ran successfully, automatically deploying updates to ECS.

---

## Challenges Faced

- Misconfigured IAM roles for CodeBuild preventing ECR access  
- Mismatched container names between ECS and `buildspec.yml`  
- Missing environment variables causing deployment errors  

---

## Key Takeaways

- Automation is powerful but requires precise configuration  
- Proper IAM permissions are essential for AWS CI/CD workflows  
- Infrastructure planning with VPC, subnets, and load balancers ensures scalable and secure deployments  
- Troubleshooting is part of learning; each fix strengthens understanding  

---

## License

This project is licensed under the MIT License.  

---

