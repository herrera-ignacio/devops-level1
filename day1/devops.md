# Intro to DevOps

## DevOps

DevOps is a mix of patterns intended to __improve collaboration__ between Development and Operations. It addresses __shared goals and incentives, as well as processes and tools__. It respects the fact that companies and projects have specific cultures and that __people are more important that processes, which in turn, are more important than tools__.

### Why?

* Development and Operations are often in __conflict__ with each other.
	* Development wants to see their changes delivered to client quickly, whereas Operations are interested in stability, which means not changing the production systems too often.
* DevOps describes practices that streamline the software delivery process, emphasizing the learning by streaming feedback from production to Development and improving the cylcle time.
* DevOps will not only empower you to deliver software more quickly, but it will also help you produce higher-quality software that is more aligned with individual requirements and basic conditions.

### How DevOps help?

* DevOps means to __close Development and Operations gaps by aligning__ incentives and sharing approaches for processes and tools, broaden the usage of Agile practices to Operations to foster collaboration and streamline the entire software delivery process in a __holistic way__.

* Please __do not think of DevOps as a new tool for eliminating Operations staff or as a tool suite__, rather it's an approach for freeing up time of the current staff to focus on harder problems that can deliver even more business value.

* DevOps may bring together subprocess to form a comprehensive delivery process that enables software to be delivered at high speed and with high quality. However, DevOps does not necesarily introduce new tools. A specific new tool can be used to implement a given process.

* __DevOps is about discipline, conventions, and a defined process that is transparent for all__.

### What is not DevOps

* DevOps does not allow developers to work in produciton system. It is not a "free" process that opens production-relevant aspects to developers. Instead,

* Similary the opposite is nether true. DevOps does not mean that Operations experts take over all Development tasks.

* DevOps is not a new department. Every attempt to establish a DevOps-type department leads to bizarre constructions.

* DevOps is also not a new role profile that will supplant current developers and operations experts.

* Some people make the false statement that "DevOps is a catalog of recipes, implement them all, and you are all done". This is false because you will focus on finding the best solution to your individual situation. There is no one-size-fits-all solution and no "DevOps by the book" approach that will solve your problems.

### Building blocks of DevOps

#### Measurements and Metrics

Measuring what you are doing is a crucial aspect of software engineering. Sooner or later, you'll have to decide on which metrics you want to use during your software engineering process, consider which metric is meaningful enough to aid all participants, as well as the Development and delivery processes.

Both traditional and agile prjects often emphasize the importance of measurement because you can only improve if you measure.

* Traditional projects emphasize measurement as an important tool for tracking progress, identifying the current status and scheduling dates.
* Agile projects settins try to find different approaches to create measurements, but often find themselves on dead-end streets when trying to bridge Operations to Development.

#### Improve Flow of Features

* DevOps is essentially about gaining fast feedback and decreasing risk of releases through a holistic approach that is meaningful for both Development and Operations.
* One major step for achieving this approach is to improve the flow of features from their inception to their availability. This process can be refined to the point that it becomes important to reduce batch size without changing capacity or demand.

#### Improve and Accelerate Delivery

After a release, involing a team spending a weekend in a data center deploying the past three months of work, the last thing anybody wants to do is deploy again anytime soon.

* __If a task is painful, the solution is to do it more often and bring the pain forward__.

* Choosing __small releases over big releases__ eventually delivers the same amount of functionality, but more functionality is delivered more quickly, and software will return value more quickly too.

* __Deploying to production frequently will help keep things simple and make individual changes more focused__. The __risk of deployments is reduced__ because you practice the process of deployments.

* By bring the pain forward, __you'll identify problems in your process and tool chains earlier__, and you will be able to optimize accordingly. As a result, the deployment itself will also only change in smaller batches between diferent deployments.

* The process of __fixing incidents will also become optimized__, as changes between deployments are small, and in turn helps with learning about the root causes of production incidents. __Errors can be fixed faster__, and that makes a total rollback unnecesary, but even if you have to roollback, you only need to do so over a small set of changes.

* It is not only a technical improvement, but also a more managable situation from a business viewpoint, to rollback a single feature than a full release with hundreds of features.

#### Automation

* Automating common tasks in building, testing, and releasing software helps to __increase efficiency and set up a reproducible process__ that can be implemented by tool chains. We can even automate the provisioning and deployment of the virtual machines and middlewares. This __ensures repeatibility__.

* Automating tasks such as integrating, build, packaging, and deployment steps will __facilitate rapid iterative development__.

* Do not automate simply because you want to. __It's bad if your automation activities are driven by technical instead of business considerations__.

* Automation is performed to gain fast feedback. __Efficient automation makes humans more important__ not less. Be always aware of the consequences and pitfalls of automation.

* Law of marginal costs
* Verb/noun mistake (processes vs artifacts, e.g. test)
* Paradox of automation

## Team Settings

### Traditional team

* Hero cult
* Emphasis on titles
* Shadow responsibilities
* Favor a plan over planning
* Operations accompanies and accounts for "the last mile" (waterfall-like scenario)
* Cultural barriers
	* Separated teams
	* No common languages
	* Fear (others activities impact, no shared goals or incentivies, reputation, etc)

### Agile team

* One team approach
	* Quality and QA is responsibility of the whole team
	* Everyone helps programmers understand what to code
	* Project roles instead of separated teams
* Operations often acts or gets tracted as a silo
	* Tasked with deliverables from development to make software available to users
	* Often receives non-functional requirement after development that may conflict with shipped software
* Blame game (conflicts during/after deployment, conflicts about performance, etc)

* The __advantages of Agile processes are often nullified__ because of the obstacles to collaboration, processes, and tools that are built up in front of Operations. As a result, __software is release frequently but only in test environments__ and rarely brought to the production phase. Consequently, not all releases are production-ready, and in sum, the cycle time from the inception of an idea to its delivery to users is still too long

* High frequency of software releases only on development and the incentives for Operations leads to what many people in the __company will perceive as a bottleneck at Operations__.

* __Developments may have finished the functionality in fast cycles by using Agile frameworks__, but operations may not want or be unable to receive all the changes in fine-grained portions.

* Development will blame operations for not being able or willing to set the software live. Operations will blame the Development team members because of their inability to develop production-ready software.

### DevOps team

* The conflict between Development and Operations __worsened when Development began adopting more Agile processes__. Iterative models such as Scrum, have become mainstream, but all too often, companies have stopped adding people and activities to the Scrum process at the point where software releases were shipped to production.

* The __one team approach__ should also include expert from Operations who develop, for instance, automation scripts or _Infrastructure as code_.

* Shared goals and values, sense of collective ownership.

* Allignment of incentives, processes and tools.

* DevOps is about __team play__ and a __collaborative problem-solving approach__. If a service goes down, everyone must know what procedures to follow to diagnose the problem and get the system up and running again. Additionally, all roles and skills necessary to perform these tasks must be available and able to work together well. __Training__ and __effective collaboration__ are critical here.