# ECS: Elastic Container Service

Deploy, manage, and scale Docker containers running applications, services, and batch processes.

It integrates with other resources like

* ELB: ELastic Load Balancing
* EC2 Security Groups
* EBS Volumes
* IAM Roles

## Features

* Can host cluster on a __Serverless__ infrastructure (Fargate).
* Can host cluster over your infrastructure (EC2).
* Schedule placement of containers across your cluster based on policies.
* More fine-grained control than Elastic Beanstalk.

## Configuration

### Get a Docker Image

![docker image](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containers.png)

We'll use [Docker Hub](https://hub.docker.com/_/registry), as it is a good solution for all cloud providers, but it's worth mentioning that cloud providers usually provide their own Docker Registry, in the case of AWS, ECR: Elastic Container Registry. They all work the same, through CLI.

Docker Hub -> Repository -> Create Private Repo

Now we go to the our own project and build the docker image of the application.

```
docker build -t ignacioherrera/amalganode:latest -f Dockerfile ./
```

Then we need to push it to our private registry
 
```
docker login -u <user> -p <pwd>
```

And push the image

```
docker push ignacioherrera/amalganode:latest
```

In case of a private repo, the blueprint is the following:

```
docker login <REGISTRY_HOST>:<REGISTRY_PORT> -u <USER> -p <PWD>
docker tag <IMAGE_ID> <REGISTRY_HOST>:<REGISTRY_PORT>/<APPNAME>:<APPVERSION>
docker push <REGISTRY_HOST>:<REGISTRY_PORT>/<APPNAME>:<APPVERSION>
```

### Task Definition

Text file, in JSON format, that describes one or more containers, that form your application.

* Task Execution IAM Role -> Required to pull images and publish container logs to CloudWatch on your behalf.

ECS -> Task Definition -> Task Role -> Container -> Private Authentication (Secret Manager)
	* Secret Manager -> Other types of secrets -> secret ARN -> Plain text
	* Container (Dockerhub) -> `docker.io/user/repository:tag`
	* Port Mapping
	* Environment Variables
		* We'll do a test without env variables to experiment debugging

#### Secret Manager for Dockerhub

```json
{
  "username" : "privateRegistryUsername",
  "password" : "privateRegistryPassword"
}
```

### Task Scheduling

A _Task_ is the instantiation of a _Task Definition_ within a cluster. There are several scheduling options available, a common one is defining a __Service__ that runs and maintains a specified number of tasks simultaneously.

![services](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-service-fargate.png)

### Cluster

Tasks are placed on a __Cluster__, which s a logical grouping of resources.

ECS -> Cluster -> Create Cluster -> EC2 Linux + Networking

	* About CloudFormation

#### Services

Service lets you specify how many copies of your task definition to run and maintain in a cluster.

* Created Cluster -> Create Service -> Task Definition
* Service AutoScaling
* [About Healthy percent](https://stackoverflow.com/a/40741816)


### Container Agent

The _Container Agent_ runs on each infrastructure resource within an Amazon ECS Cluster. It sends information about the resource's current running tasks and resource utilization to Amazon ECS, and starts/stops tasks whenever it receives a request from Amazon ECS.

![container agent](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-containeragent-fargate.png)

