# Running Cloud Servers

![cloud servcers](https://i.pinimg.com/236x/ec/e9/e5/ece9e5ab8500e8e3a5601a95c7d5d7f2--cloudcomputing-heavens.jpg)

First we need something to run. We'll use a demo http api with Node.js, and we'll start building on top of that.

First, we only have some basic HTTP Routing to process GET requests.

* [Code](https://github.com/herrera-ignacio/code-example)

## Creating the servers

![wizard cloud](https://smidcloud.com/wp-content/uploads/2018/01/4-300x300.jpg)

* EC2 -> Launch Instance -> AMIs
* Different AMIs options, we'll go with Quick Start for now

### Quick chat about Instance Types

![cloud scientific](https://i.pinimg.com/236x/64/08/32/64083250e48223e94387ee3ba45c09c6.jpg)

Instance types are optimized to fit different use cases, varying combinations of CPU, memory, storate, and networking capacity, for the flexibility to choose the appropriate mix of resources for your application.

[Let's read the docs!](https://aws.amazon.com/ec2/instance-types/)

### Further Configuration

![aws bill comming](https://miro.medium.com/max/1244/0*u5gUyhB0YFd3VzWM.jpg)

* Burstable instances (T)
* Spot instances, great for development!
* Placement group (logical clusters)
* [User Data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)
* [Storage types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Storage.html?icmpid=docs_ec2_console)
	1. Instance Store: disk that is physically attached to host computer, temporary block-level (persists only during life of the associated instance, until stop or terminated)
	2. EBS: attachable block-level storage volumes, recommended for database on an instance
	3. EFS: scalable file storage for workloads and applications running on multiple instances.
	4. S3: any amount of data at any time, from within EC2 or anywhere on the web.


### User Data Example

![no idea](https://wpctrl.b-cdn.net/wp-content/uploads/2020/01/dog-hosting-meme.jpg)

Create a configure a LAMP server with a bash cript.

```
#!/bin/bash
yum update -y
amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
yum install -y httpd mariadb-server
systemctl start httpd
systemctl enable httpd
usermod -a -G apache ec2-user
chown -R ec2-user:apache /var/www
chmod 2775 /var/www
find /var/www -type d -exec chmod 2775 {} \;
find /var/www -type f -exec chmod 0664 {} \;
echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
```

Do the same with _cloud-init_ directives.

```
#cloud-config
repo_update: true
repo_upgrade: all

packages:
 - httpd
 - mariadb-server

runcmd:
 - [ sh, -c, "amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2" ]
 - systemctl start httpd
 - sudo systemctl enable httpd
 - [ sh, -c, "usermod -a -G apache ec2-user" ]
 - [ sh, -c, "chown -R ec2-user:apache /var/www" ]
 - chmod 2775 /var/www
 - [ find, /var/www, -type, d, -exec, chmod, 2775, {}, \; ]
 - [ find, /var/www, -type, f, -exec, chmod, 0664, {}, \; ]
 - [ sh, -c, 'echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php' ]
```

### AMIs

![dr](https://www.aspiretech.com/images/easyblog_shared/October_2016/10-7-16/ib_meme_4.jpg)

Right click instance -> Create Image :)

## Deploying our application manually

![delivery](https://hackernoon.com/hn-images/1*8wcrKrzgaEJwNKam8LCYCw.jpeg)

1. SSH into instance
2. We need to install lots of things...

```
sudo yum update -y
sudo yum install git
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 12 --lts
nvm current
node -e "console.log('Running Node.js ' + process.version)"
```

In this case, we'll need to run node server as sudo, and we need to do some tweaks to let sudo know where node is. We'll create a symbolic link as a workaround.

```
whereis node
sudo ln -s <NODE_PATH> /usr/bin/node
```

3. Now we are ready to pull the repo!

```
git clone https://github.com/herrera-ignacio/code-example.git
```

4. Run the server :)

```
npm install
npm run dev
```

5. Go to Instances -> Instance Description -> Public DNS (IPv4)

![git push](https://hackernoon.com/hn-images/1*GwoUrficuzqGSxXD-gztEQ.jpeg)
