# Udagram Image Filtering Microservice

## Introduction

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into three parts:
1. [The Simple Frontend](/udacity-c3-frontend)
A basic Ionic client web application which consumes the RestAPI Backend. 
2. [The RestAPI Feed Backend](/udacity-c3-restapi-feed), a Node-Express feed microservice.
3. [The RestAPI User Backend](/udacity-c3-restapi-user), a Node-Express user microservice.

## Instructions

### Setup Docker Environment
You'll need to install docker https://docs.docker.com/install/. Open a new terminal within the project directory and run:

1. Switch the folder: `cd udacity-c3-deployment/docker`
1. Build the images: `docker-compose -f docker-compose-build.yaml build --parallel`
2. Push the images: `docker-compose -f docker-compose-build.yaml push`, for integrating changes
3. Run the container: `docker-compose up`, to confirm that the project runs locally

### Setup k8s Environment
Setting up the k8s cluster can be done either via the AWS console or through command terminal. For the specific steps followed by this project:

1. Create an EKS cluster (as well as node groups) via the AWS console
2. The following tools must be installed: [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) and [aws-iam-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)
3. Set up [kubeconfig](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html)
4. Load the YAML files from: `kubectl apply -f udacity-c3-deployment/k8s`, to create deployment and service

### Port-forward using k8s services
Once the EKS cluster is populated with services and deployments, it is possible to serve the application on localhost by:

1. Run `kubectl port-forward service/reverseproxy 8080:8080`
2. Run `kubectl port-forward service/frontend 8100:8100`

### TravisCI
If changes need to be made to the features:

1. Make changes to the code and commit
2. Run `git push` to trigger the ci/cd-pipeline
3. The pipeline will re-build all the latest images and push them to [this docker hub](ttps://hub.docker.com/u/rebekkahaley)
4. Currently, the EKS cluster will need to be manually updated via: `kubectl apply -f udacity-c3-deployment/k8s`

## Appendix

### Resources used:
- [Why isnt "kubectl apply" working?](https://stackoverflow.com/questions/60379799/host-not-found-in-upstream-when-using-kubectl-apply-f-but-works-in-doc)
- [How to fix "invalid ELF header" with Docker and Bcrypt](https://medium.com/@devontem/solved-invalid-elf-header-with-docker-and-bcrypt-444426d63605)