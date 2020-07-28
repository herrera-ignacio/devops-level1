# Elastic Beanstalk

## IaaS

* Capacity provisioning
* Load balancing
* Autoscaling from Health Monitoring
* NO ADDITIONAL CHARGE (pay for resources)

## Developer Productivity

* Provisions and operates infrastructure
* Keep underlying platform up-to-date
* Focus on writing code

## EB vs ECS

EB is an abstraciton layer on top of ECS, with some bootstrapped features and some limitations.

* Automatically interacts with ECS and ELB
* Cluster health and metrics are readily available
* Load balancer must terminate HTTPS and all backend connections are HTTP
* Easily adjustable autoscaling
* Manageable environment variables
* Less control over application scaling and capacity.
* All cluster instances must run the same set of containers.
	* Bad if you want to schedule a replicated set of queue workers
	* ECS is better for wokrflows that involves schedulers to run batch workloads.
* Manages automatic recovery without the need of AutoScaling Groups as ECS.

## Hands On

_Disclaimer_: We'll do some fast-forwarding in practice, because Elastic Beanstalk deployment takes some time!

[Tutorials](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html)
	* [Multi-container app with `Dockerrun.aws.json`](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_ecstutorial.html)


We can deploy using a `Dockerrun.aws.json` file. For private repositories, we'll need to setup credentials using an S3 bucket, [check documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.container.console.html#docker-images-private).

To go faster, we'll use a public repository.

First, we need to install de EB CLI

```
brew update
brew install awsebcli
eb --version
```

We use EB CLI to configure our project for deployment

```
eb init -p docker <name>
eb create // create environment (we can change envs with -> env use)
eb status // check
eb open // check
eb deploy // update
eb terminate --force
```

And we need to setup environment variables

Environments -> <env_name> -> Configuration -> Software 
