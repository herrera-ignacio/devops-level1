# Day 3

![here we go again](https://media1.giphy.com/media/Q7dPLqwJN1mK3Dm4h6/giphy.gif)

* [Running cloud servers!](./ec2.md)
* [Data Persistence](./persistence.md)

## Problems so far

![jira](https://pics.me.me/thumb_i-will-find-you-and-i-will-assign-a-jira-50240376.png)

### 1. Deploying changest fast

![deadlines](https://community.atlassian.com/t5/image/serverpage/image-id/24151i6B6D7FF05434BBF0/image-size/large?v=1.0&px=999)

* Need to manually install dependencies every time
* Set up environment & configurations files manually

#### Solution

* AMIs
* Docker
* CI/CD

### 2. Scalability & Reliability?

![scalability](https://www.memecreator.org/static/images/memes/4788458.jpg)

* What if we have an increasing throughput?
* What if we need to rapidly turn off idle resources? -> Problem 1
* What if server crashes? (Exceptions, or maybe just EC2 terminating)

#### Solution

* Process Managers (PM2)
* Clusters (Docker-Swarm, Kubernetes)
* AutoScaling Groups
* ECS (next class)

### 3. Security

![Am I a joke to you?](https://i.imgflip.com/1luyae.jpg)

* Public available databases with default credentials and ports
* S3 resources public available for all users with urls
* SSH ports opened

#### Solution (Out of scope)

![network security](https://www.memecreator.org/static/images/memes/4972861.jpg)

* VPC & Security Groups
* No SSH, open for immediate use and then close, only for your IP
* Mask bucket with cloudfront, and restrict access (whitelist > blacklist).
* Don't allow user input in file names
	* Conflicts
	* XSS
* We are implicitly responsible with the client.
	* We must be complient with common-sense
	* The client expects professional advice from us
	* Let the client decide
	* +Security = More features = More work = $$$
	* -Security = Possible outage = -$ if not worse
