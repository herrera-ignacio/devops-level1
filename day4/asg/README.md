# ASG: Auto Scaling Groups

![ASG](https://media.giphy.com/media/12BYUePgtn7sis/giphy.gif)

## Multiple Ways

* From Launch Template
* From Launch Configuration
* Using an EC2
* (Advanced) Multiple instance types and purchase options

## Using Launch Template

* AMI
* Instance Type
* Security Groups
* Advanced -> User Data

## Using Launch Configuration

* AMI
* Instance Type (Spots)
* Advanced Data -> User Data
* Security Groups

### Launch Template vs Launch Configuration

![coding](https://media.giphy.com/media/LmNwrBhejkK9EFP504/giphy.gif)

Launch templates are

* Versioned
* Integrated with more services, such as EC2, AWS SDK, and CLI.

## Using an EC2

![ec2](https://media.giphy.com/media/NiU9MQUOSaOPe/giphy.gif)

Amazon EC2 Auto Scaling creates a launch configuration for you and associates it wih the Auto Scaling group. It derives its attributes from the specified instances, such as AMI ID, instance type, and availability zone.

### Limitations

* Tags are not copied from instance to new ASG.
* ASG includes block device mapping from the AMI used to launch the instance, but it does not include any block devices attached after instance launch.
* If identified instance is registered with one or more load balancers, the information about load balancer is not copied to the load balancer or target group attribute of the new ASG.

### Hands on


EC2 Instance -> ASG from running EC2 instances -> add instance to new ASG.
	* Instance -> Actions -> Instance Settings -> Attach to ASG (New)
	* (OPTIONAL) Auto Scaling -> ASG -> Edit

You can attach new instances to the ASG:

* Instance -> Actions -> Instance Settings -> Attach to ASG -> existing ASG

## Must know

![hands on](https://media.giphy.com/media/cFkiFMDg3iFoI/giphy.gif)

### ELB: Elastic Load Balancing

Is used to automatically distribute incomming application traffic across all EC2 instances you are running, so that no one instance is overwhelmed.

Your ELB acts as a single point of contact for all incoming web traffic to your ASG.

Instances launched by your ASG are automatically registered with the load balancer. Likewise, instances that are terminated by your ASG, are automatically deregistered from the load balancer.

* Target groups (Load Balancing -> Target Groups)
	* Tells a load balancer where to direct traffic to. Load balancer will route requests to registered targets.
	* Instances, IP addresses or Lambda Function.
	* [Example of NLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-target-group.html)
	* Each load balancer node checks the health of each target, using the health check settings for the target groups with which the target is registered.
* Load Balancers (ELB)
	* ALB: Application (HTTP/S), routing and visibility features targeted at app level, good for microservices and containers.
	* NLB: Network (TCP, TLS, UDP), ultra low latencies.
	* Classic Load Balancer (deprecated)

### Health Checks

* EC2 automatically replaces instances that fail health checks (must basic usage).
* ELB health checks

### Scale in

* Termination policies
* Suspended Processes

### Scaling Policies

ASG -> Select one -> Automatic Scaling

* Dynamically scale EC2 capacitity, based on demand.
* Default alarms or alarms in Cloudwatch and define two policies
	* [Docs](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-monitoring.html)
	* Scaling out
	* Scaling in
