# Codefresh GKE plugin

Use Codefresh GKE plugin to create GKE Kubernetes cluster for integration tests 


## codefresh/plugin-gke Docker Image details

### Requirements:
  set GOOGLE_SERVICE_ACCOUNT_KEY to the Google Service Account Key value
  Service account should have enough permissions to create and operate GKE Cluster:
    Kubernetes Engine Admin or subset of privileges from GKE Developer+clusterAdmin  

### Environements:
  - GOOGLE_SERVICE_ACCOUNT_KEY - Google Service Account Key value
  - GKE_CLUSTER_NAME - name of gke cluster to create  
  
  - Codefresh variables: https://docs.codefresh.io/docs/variables

### Commands: 

* `gke-create` - creates GKE cluster on Google Cloud project defined by GOOGLE_SERVICE_ACCOUNT_KEY
  - `gcloud container clusters create <GKE_CLUSTER_NAME> <optional parameters>`
  - sets kubectl current context to the newly created cluster for futher pipeline steps

* `gke-delete` - deletes GKE Cluster by name on Google Cloud project defined by GOOGLE_SERVICE_ACCOUNT_KEY  
  
* `kubectl` - you can use it to operate on the created cluster 
* `gcloud`

## Usage

Set environment variable and add the following step to your Codefresh pipeline:

```yaml
---
version: '1.0'

steps:

  ...

  # 
  create-cluster:
    image: codefresh/plugin-gke
    commands: 
        - gke-create --num-nodes 2 --machine-type n1-standard-2
    
  deploy-my-service:
    image: codefresh/plugin-gke
    commands:
        - deploy.sh
        - kubectl run --image mytestimage
        - check-status.sh

  clean:
     image: codefresh/plugin-gke
     commands:
        - gke-delete
  ...

```