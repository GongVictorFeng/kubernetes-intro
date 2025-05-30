# kubernetes Introduction

## Container Orchestration
  * Requirement: There are 10 instances of Microservice A container, 15 instances of Microservice B container and ...
  * Typical Features:
    * Auto Scaling - Scale containers based on demand
    * Service Discovery - Help microservices find one another
    * Load Balancer - Distribute load among multiple instances of a microservice
    * Self Healing -Do health checks and replace failing instances
    * Zero Downtime Deployments - Release new versions without downtime

## Container Orchestration Options
  * AWS Specific
    * AWS Elastic Container Service (ECS)
    * AWS Fargate: Serverless version of AWS ECS
  * Cloud Neutral - Kubernetes
    * AWS - Elastic Kubernetes Service (EKS)
    * Azure - Azure Kubernetes Service (AKS)
    * GCP - Google Kubernetes Engine (GKE)

## Kubernetes 
  * Most popular open source container orchestration solution
  * Provides Cluster management (including upgrades)
    * Each cluster can have different types of virtual machines
  * Provides all important container orchestration features
    * Auto Scaling
    * Service Discovery
    * Load Balancer
    * Self Healing
    * Zero Downtime Deployments

## Google Kubernetes Engine (GKE)
  * Manage Kubernetes service
  * Minimize operations with auto-repair (repair failed nodes) and anto-upgrade (use latest version of kubernetes) features
  * Provides Pod and Cluster Autoscaling
  * Enable Cloud Logging and Cloud Monitoring with simple configuration
  * Uses Container-Optimized OS, a hardened OS built by Google
  * Provides support for Persistent disks and Local SSD

## Thirteen Steps to create a cluster and deploy a microservice
1. Create a Kubernetes cluster with the default node pool 
    * gcloud container clusters create or use cloud console
2. Login to Cloud Shell
3. Connect to the Kubernetes Cluster
   * gcloud container clusters get-credentials my-cluster --zone us-central1-c --project evident-axle-461419-m9
4. Deploy Microservice to Kubernetes
   * Create deployment & service using kubectl commands
     * kubectl create deployment hello-world-rest-api --image=gongvictorfeng/hello