# Refactoring Monolithic App into Microservices Project Submission Notes

## Containers and Microservices
The project has been split into microservices:
- feed
- user
- frontend
- reverse proxy

DockerHub images for each microservice:

![dockerhub_updated_images](dockerhub_updated_images.PNG)


## Independent Releases and Deployments
Each project includes a .travis.yml file. Although this currently isnt needed as there are no tests. DockerHub provides its own automated CD system.

Travis automatically triggered build upon push:

![travis_ci_trigger_builds.PNG](travis_ci_trigger_builds.PNG)

Travis successful deploy to DockerHub:

![travis_ci_docker_push.PNG](travis_ci_docker_push.PNG)

DockerHub receives updated tagged image from Travis:

![dockerhub_new_tagged_image.PNG](dockerhub_new_tagged_image.PNG)

## Service Orchestration with Kubernetes
Kubernetes hosts API, Reverse Proxy and Frontend Web Server (note that the feed api has 2 replicas):

![kubectl_get_pods.PNG](kubectl_get_pods.PNG)

Kubectl Services do not expose any sensitive variables. All environment variables are passed in through secrets and config maps:

![kubectl_describe_services_1.PNG](kubectl_describe_services_1.PNG)

![kubectl_describe_services_2.PNG](kubectl_describe_services_2.PNG)

Kubernetes /feed service is replicated. Deployment.yml:

![k8_deployment_replicas.PNG](k8_deployment_replicas.PNG)

Furthermore there is autoscaling configured:

![kubectl_get_hpa.PNG](kubectl_get_hpa.PNG)

# Debugging, Monitoring, and Logging
Logs when user registers:

![pod_user_logs.PNG](pod_user_logs.PNG)

Logs when user uploads to feed:

![pod_feed_logs.PNG](pod_feed_logs.PNG)

## Dependancy Graph of Application Services and AWS Resources
The overall architecture/dependancy graph of the web service:

![architecture](diagram_architecture.PNG)

## Setting up CORS
The backend has been setup with cors, such that it only allows the browser to send requests when the browser is on the host's website.

CORS Rejection (before setting up CORS):

![cors_rejection.PNG](cors_rejection.PNG)

Setting up CORS:

![cors_setup.PNG](cors_setup.PNG)

CORS Acceptance (after setting up CORS):

![cors_acceptance.PNG](cors_acceptance.PNG)