## Refactored Udagram Microservice

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into three parts:
1. [The Simple Frontend](/udacity-c3-frontend)
2. [The RestAPI Feed Backend](/udacity-c3-restapi-feed)
3. [The RestAPI User Backend](/udacity-c3-restapi-user)

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
4. Load YAML files: `kubectl apply -f udacity-c3-deployment/k8s`, to create deployment and service

### Serve Application via port-forward
Once the EKS cluster is populated with services and deployments, it is possible to serve the application on localhost by:

1. Run `kubectl port-forward service/reverseproxy 8080:8080`
2. Run `kubectl port-forward service/frontend 8100:8100`

### TravisCI
The integration of Docker images is handled through:

1. Change git repo code and commit
2. Run `git push` to trigger the ci/cd-pipeline
3. The pipeline will re-build all the latest images and push them to [this docker hub](https://hub.docker.com/u/rebekkahaley)
4. Currently, EKS cluster will need to be manually updated with the latest images: `kubectl apply -f udacity-c3-deployment/k8s`

## Appendix

### Resources used:
- [Why isnt "kubectl apply" working?](https://stackoverflow.com/questions/60379799/host-not-found-in-upstream-when-using-kubectl-apply-f-but-works-in-doc)
- [How to fix "invalid ELF header" with Docker and Bcrypt](https://medium.com/@devontem/solved-invalid-elf-header-with-docker-and-bcrypt-444426d63605)
- [Continuous delivery to Kubernetes with Travis CI](https://caveofcode.com/continuous-delivery-to-kubernetes-with-travis-ci/)