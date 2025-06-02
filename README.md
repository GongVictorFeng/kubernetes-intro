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

## Fourteen Steps to create a cluster and deploy a microservice
1. Create a Kubernetes cluster with the default node pool 
    * gcloud container clusters create or use cloud console
2. Login to Cloud Shell
3. Connect to the Kubernetes Cluster
   * gcloud container clusters get-credentials my-cluster --zone us-central1-c --project evident-axle-461419-m9
4. Create a docker image  and push it to docker hub
   * gongvictorfeng/hello-word-java:0.0.1-SNAPSHOT
   * docker push gongvictorfeng/hello-world-java:0.0.1-SNAPSHOT
5. Deploy Microservice to Kubernetes
   * Create deployment & service using kubectl commands
     * kubectl create deployment hello-world-java --image=gongvictorfeng/hello-world-java:0.0.1-SNAPSHOT
     * kubectl expose deployment hello-world-java --type=LoadBalancer --port=5000
6. Increase number of instances of the microservice:
   * kubectl scale deployment hello-world-java --replicas=3
7. Increase number of nodes in Kubernetes cluster:
   * gcloud container clusters resize my-cluster --node-pool default-pool --num-nodes=2 --zone=us-central1-c
8. Setup auto-scaling for the microservice:
   * kubectl autoscale deployment hello-world-java --max=4 --cpu-percent=70 
     * Also called horizontal pod autoscaling -HPA - kubectl get hpa
9. Setup auto-scaling for the  Kubernetes Cluster
   * gcloud container clusters update cluster-name --enable-autoscaling --min-nodes=1 --max-nodes=10
10. Add some application configuration for the microservice
    * Config Map - kubectl create configmap hello-world-java-config --from-literal=RDS_DB_NAME=todos
11. Add password configuration for the microservice
    * Kubernetes Secrets - kubectl create secret generic hello-world-java-secrets-1 --from-literal=RDS_PASSWORD=dummytodos
12. Deploy a new microservice which needs nodes with a GPU attached 
    * Attach a new node pool with GPU instances to the cluster
      * gcloud container node-pools create POOL_NAME --cluster CLUSTER_NAME 
      * gcloud container node-pools list --cluster CLUSTER_NAME
    * Deploy the new microservice to the new pool by setting up nodeSelector in the deployment.yaml
      * nodeSelector:cloud.google.com/gke-nodepool:POOL_NAME
13. Delete the Microservices
    * Delete service - kubectl delete service
    * Delete deployment - kubectl delete deployment
14. Delete the cluster
    * gcloud container cluster delete

## Google Kubernetes Engine (GKE) Terminologies
  * Cluster: Group of Compute Engine instances
    * Master Node(s) - Manages the cluster
    * Worker Node(s) - Run the workloads (pods)
  * Master Node (Control Plane) components:
    * API Server - Handle all communication for a kubernetes cluster (from nodes and outside)
    * Scheduler - Decides placement of pods
    * Control Manager - Manages deployments & replicasets
    * etcd - Distributed database storing the cluster state
  * Worker Node components:
    * Runs pods
    * Kubelet - Manages communication with master node(s)