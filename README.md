# System design 999

## Context

- This document is my take on concepts around system design
- This is in no ways a doucment only meant for interview purposes
- Aim is to educate budding engineers and wannabe architects on concepts around system design, but helps with interviews too
- Attempt is to highlight patterns instead of specific technologies to build a mental model of the concept
- Mainly technology agnostic but in some cases technologies are used for example and illustration purposes
- Concepts are generally post 2020. Any references to previous architectures is just for comparison purposes

## Architecture v/s Engineering

What is achitecture? What is engineering?
Some say they are the same, some say they are distinct, sme feel they overlap

Here are my thoughs. They surely have some overlap

### Architecture - Patterns + Technology

Patterns are reoccuring concerns and solutions in the technology world.
Technology is used to impement the above pattern

For example - Content delivery network
The pattern is based around caching. It is implemented using technologies like AWS Cloudfront

### Engineering - Implementation patterns

Once the above architecture is defined then Engineering defines patterns around the implementation of it.
This includes but not restricted to

1. Git strategies
2. Coding standards
3. Naming conventiones for components
4. Build and Deploy
5. DevOps
6. SRE ...


## A few concepts unique to this article

**H**istorically a lot of names have been used to call different components i prefer using a 
few terms which have become most popular post 2020 times

- **Client** - Represents a UI component visible to the end user, could be a mobile App or UI using React js, etc
- **Compute**- Represents back end processing, such as services, cache and queue, generally of no significance to the end user
- **Persistence** - Something which can store data for long term (Basically databases)


## Abstract concepts

### The holy SixTrait of technology

Any technology uses underlying infrastructure and runtime
Any logical component (Refer below section [Worfklows and components]) needs the below to be able to do something meaningful

![The holy SixTrait of technology](https://github.com/saurabh-herwadkar/system-design-999/blob/main/images/The%20holy%20SixTrait%20of%20technology.png)



**Hardware **

- Processor - CPU or GPU
- Memory - RAM
- Storage - SSD or HDD
- Input/Output (IO) - Network cards, Display cards

**Software**

- Operating system | Kernel - Some variant of Linux or Windows which gives access to the above 4 mentioned hardware components via 
- Runtime - The underlying installed programmin softwares - For example Python 3.9, PIP and other python libraries

  To add to the above we have the code base which executes some bsuiness functionality and cannot run
  without the blessings of the above mentioned "Sixtrait"


### Concept , features and tools

**W**henever i work with an engineer to discuss an idea i use the above three key words to helpp build a mental model

- **Concept** - Understand the underlying concept at an abstract level in a technology agnostice manner
- **Features** - What are the properties and features of that concept
- **Tools** - What are the industry grade tools which help implement this concept

To take the example of a queue mechanism 

- **Concept** - Queue - A mechanism where a publisher can send messages. A consumer can consume these messages asynchronously
- **Features** -Publisher | Subscriber | Asynchronous | Decoupled | FIFO queues | Dead letter queues | Message retention
- **Tools** - Active MQ, Rabbit MQ, AWS SQS

### Worfklows and components

**N**ow for this definition I have leaned a bit on AWS take on the above two terms

[https://aws.amazon.com/what-is/workflow/]

A **workflow** is a business process which achieves a business objective. 
- It brings value
- It surely incurrs a cost
- Yet reduces overall costs
- Increases profits

A **component** is a logical unit of software
Many such **components** work together to complete a **Workflow**

For example a UI client, microservice and database **components** work together to achieve a user registration **workflow**

A few examples of components are

| Component Category | Example           
| :-------------:|:-------------:| 
| Client | IOS app, Android app, Reactive app, Progressive web app, Hybrid app | 
| Compute | Microservice, Lambda | 
| Persistence | MySql database, Cassandra |
|Orchestration| Queue, Event bus, Cache|

![Worfklows and components](https://github.com/saurabh-herwadkar/system-design-999/blob/main/images/Worfklows%20and%20components.png)


### Logical component

**A**t the onset I need to clarify the concept of Logical component 

A component is basically a
(code + runtime + OS ) running on either a Container , VM or a Physical instance (Quite unlikely but possible)
A logical component may represent a Client component , or a compute component or a persistence component

For example

UI - A UI project or Mobile application
Compute - A container running a micro service
Persistence - A Mysql database
Orchestration - Queue, Event bus


### Polyglot persistence

**A** misconception is that we should ideally use only one persistence (database) technique for our information system
Different use cases (user stories) may ask for different types of database solutions within the same workflow
For example
1. Finanical transactions may require a Relational database with strong ACID properties (eg Oracle)
2. While user profiles may be ok with a No SQL databse with BASE properties (eg Mongo DB)

Modern engineers and budding Architects should be open to use different databse for different use cases within the same solution

**A** few key features are

1. Fit for purpose usage of database technologies as per us case
2. Mainly **Breaking** of the previous notion that there is only one database solution pre workflow
3. Cloud technologies and managed services make provisioning of these databases very easy

# Polyglot programming

**S**imilar to the concept of Polyglot persistence above different use cases may demand for different programming languages
Engineers should be open to using different programming languages in different components in different Micro services

For example

1. A Payment service may work best in Java Spring boot Spring MVC Container configuration + Oracle relation database (ACID)
2. An OTP generation service may fit good in Python 3, Lambda Serverless  + Mongo DB (BASE)

**A** few key features are

1. Use seperate programming language for a different use case (Possibly micro service)
2. Seperate programming languages would mean the associated underlying run times and a compatible DevOps pipeline


![Polyglot](https://github.com/saurabh-herwadkar/system-design-999/blob/main/images/Polyglot.png)


### Distributed systems

Contrary to the previous concept of having all components on a single machine and probably in a monolith
Distributed systems by default will have their components on seperate instances.
Components like UI, Compute and persistence will be on seperate logical instances

**A** few key features are

- In modern achitectures (Post 2020) by default your system will be distributed
- Communication betwweh these components will be mainly using HTTP(s) though  other protocols may apply
- This surely increase the complexity of the system overall
- This reduces the total points of failure and mitigates against a single point of failure (SPF)
- Having components on different logical isntances ensures that there is optimum usage of resources
- Addition or removal of resources can be managed by concepts such as Auto scaling 

### Monolith v/s Micro sevices

![Monolith vs Micro sevices](https://github.com/saurabh-herwadkar/system-design-999/blob/main/images/Monolith%20vs%20Micro%20sevices.png)


- One cannot talk about Micro services lest you compare it with a Monolith its ancestor
- Monolith would generally mean all the code for various functionalities were included in one code base talking to one database
- There was a singular build process and for the smallest change entire code base would be deployed. We have seen builds which took about 20 minutes to complete for a small UI change
- Micro services beat this problem by decomposing services into smaller individual projects each serving a functionality. They may be segregated domain wise based on domain driven design
- Each Micro service has its own code base, own deployment pipeline, serves a specific functinality. It may have its ownn database or share a common database

Advantages:

- Each project is deployed fast and independently for a small change
- For a change in a specific area, indivdual project deployment is sufficient, all projects need not be deployed
- Deployement are surely faster and independent
- All services talk to each other using HTTP with a specific API contrat which does not change

Disadvantages:

- Increase complexity of the system
- If engineers do not coordinate with each other then there could be some confusion and chaos
 


### Virtual machine (VM) v/s Containers

It is very difficult to describe virtual machines and containers in isolations.They are exlpained best when compared with each other

![Virtual machine (VM) vs Containers](https://github.com/user-attachments/assets/f3b151a9-07e8-4858-b1e3-2302b74933de)


### Virtual machines

- They are fundamentally software programs which **can run multiple guest operating systems on a single physical host**
- It runs a guest operating system on top of a software called **Hypervisor** which is nothing but in itself an operating system types software
- Virtual machines do **hardware simulation** where they get access to the hardware resources via the way of the Hypervisor
- VMs appeared around in the 2000s and had settled in a matured manner by 2010
- VM are generally around 2-8 GB in size and since they do hardware virtulization they take minutes to boot

Advantages

- Best possible utilization of a servers resources by running multiple Virtual machines on the server and avoiding wastage of hardware resources
- Ability to stop, start or even shift virtual machines as per requirement
- Cost optimization


### Containers

- Containers make operating system level isolation for a program and make it appear as if the program is the only machine running on the OS
- It works on top of a software called container engine (Example docker engine)
- A container engine allows fully isolated access for the program to the underlying OS Kernel calls which in turn gives isolated access to Hardware components
- The entire program code base plus any library dependencies can be isolated and packaged as a container image
- Once packaged many container instances can be created from the container image
- Due to its operational nature, container instances are best buddies with micro services, where the micro service code and any dependencies can be packaged as a container image and deployed in an auto scaling types configuration
- Containers appeared on the scene at around 2005 and had matured and settled in by 2015
- Containers  are generally around 200-600 MB in size and since they do only logical OS level isolation they take only seconds to boot - possibly 30 s max


### Disaster recovery


### OSI model

### TCP v/s UDP

### IP

### DNS

### CDN

### Domain driven design (DDD)


## Client

### Mobile app


### Front end


### Micro front ends

### Backend For Frontend (BFF) pattern


## Compute

### HTTP

### SSL (oh you meant TLS)

### Load balancing


### Auto scaling

### API gateway


### REST v/s GraphQ: v/s gRPC


### Long staying HTTP connections

### Long polling
 
### Websockets

### Server side events (SSE)

### Service discovery

### Caching

### Serverless


## Orchestration

### Messagebroker

### Event bus

### Streaming architecture

### Event friven architecture EDA

### SLA SLO SLI


## Persistence

### Relational databases

### Keys

### Normalization and Denormalization


### ACID

### BASE

### CAP and PARCELC

### NoSql databases

### Sharding

### Database replication

### Database federation

### Locks

### Transactions and distributed transactions

### Datalake and DataWarehouse

### Data analytics




## Agile 

### Kanbahn

### Scrum

### SAFE

### aRT



## DevOps

### Philosophy

### Key practices

### 












