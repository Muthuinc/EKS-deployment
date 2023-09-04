# AWS EKS - Deployment 

### Overview

This project automates the deployment of sample web application **Myweb-app** to the AWS EKS cluster> It also demonstrates the build and push steps in docker and deployment in K8 cluster hoster on AWS.

### Pre-requisities

Before you begin you must have these,

- Docker installed in your machine.
- Docker hub account.
- Jenkins installed and configured with plugins like git docker pipeline.
- AWS cli and Kubectl tools.
- I hope you already have an account in AWS 
  - An IAM user with admin permission (for testing purpose).
  - IAM roles for K8's [refer this](https://docs.aws.amazon.com/eks/latest/userguide/service_IAM_role.html).
  - K8's cluster in the EKS . Create the cluster and node group in the console, or with any other tools like terraform.
  - Store the IAM credentials as secret text in jenkins (Both secret access key and ID).

### Project structure

- **Jenkinsfile** defines the workflow of the job, contains the credntials as environment variables, stages like build and push the docker image and deployment in K8's.
- **Codebase** contains the application.
  - **Dockerfile** specifies the steps to create the docker image.
  - **hills** our application code.
- **K8s** has the deployment configurations.
  - **deployment.yaml** defines the application name, namespaces, replicas, image name, kind of informations
  - **service.yaml** creates a service for our application to be accessible from outside. here loadbalancer type is used.


**Jenkinsfile** automates the entire process, we access the IAM credentials as the environment variables in the pipeline, the build stage perform the docker image build process. Then it is pushed to the docker hub repo. we are updating the local kubectl to perform the k8 cluster access by updating the config file (this uses the IAM credentials), Finally deployment happens, 

```
kubectl get svc
```
The above cli command reteives the loadbalancer's DNS address. view the application:clap:


At the end we clean up the image from the local machine as apost build action.

