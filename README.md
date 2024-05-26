# Deployment Documentation

## Overview

This documentation provides a comprehensive guide on how to deploy application using Kubernetes on Amazon Web Services (AWS).

## Prerequisites

Before you begin, ensure you have the following:

-   An AWS account
-   AWS CLI installed and configured
-   `kubectl` installed
-   Basic knowledge of Kubernetes concepts


## Deploying an Application

### Step 1: Configure AWS CLI

If you haven't configured the AWS CLI yet, run:
`aws configure` 

Enter your AWS Access Key ID, Secret Access Key, Default region name, and Default output format.

### Step 2: Update kubeconfig

Update your kubeconfig file to use the cluster:
`aws eks --region us-east-1 update-kubeconfig --name mka-cluster` 

### Step 3: Build and Push Docker Image

Each push into this repository's `main` branch will trigger a process in AWS CodeBuild which is going to build a new Docker image and then publish it inside AWS ECR repository called `mka-ecr`

### Step 4: Deploy to Kubernetes

Inside of `/deployments` directory you can find Kubernetes deployment file `deployment.yaml`.

#### Initial deployment
Replace container's image inside `deployment.yaml` with the latest one, that can be found in `mka-ecr` AWS ECR repository and then apply the deployment with the following command.

`kubectl apply -f deployment.yaml` 

#### Updating already running application
Update the deployment in Kubernetes to use the new image:

`kubectl set image deployment/coworking coworking=<aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/mka-ecr:<image_tag>` 
    
Kubernetes will automatically rollout the new version of your application.

Retrieve the external IP address:

`kubectl get svc coworking` 

Access your application via the external IP provided.

## Monitoring and Logging

You can monitor the status of your deployment using:

`kubectl get deployments` 

You can check the status and readiness of your pods using:

`kubectl get pods` 

In case if status of the pod is other than Running, you can troubleshoot it either by describing the pod:

`kubectl describe pod <pod_name>`

or by viewing logs from your application pod:

`kubectl logs <pod_name>` 