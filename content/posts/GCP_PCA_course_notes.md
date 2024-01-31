---
title: "Cloud architect GCP 🚧"
date: 2023-03-24T13:39:15+08:00
author: "celine"
tags: ["google_cloud", "CA"]
categories: ["StudyNote"]
---

> *Active learning* - think and take notes
>
> *Review* - every once in a while

# Section 01

### Getting Started

+ GCP has 200+ services. This exam expects knowledge of 40+ services

+ Exam tests your **decision making abilities**:

  + Which service do you choose in which situation?

  + How do you trade-off between resilience, performance and cost while not compromising on security and operational excellence?

    > Below notes will help with making the tough choices 
    >
    > And Enable you to architect amazing solutions in GCP

### Before the Cloud

+ Example 1: Online shopping app
  + Challenge:
    + Peak usage during holidays and weekends
    + Less load during rest of the time
  + Solution (before the Cloud)
    + PEAK LOAD provisioning : Procure (buy) infrastructure for peak load

+ Example 2 - Startup

  + Challenge:
    + Startup suddenly becomes popular
    + How do you handle the sudden increase in load?
  + Solution (before the cloud)
    + Procure infrastructure assuming they would be successful

  ### Challenges

  + High cost of procuring infrastructure
  + Needs ahead of time planning (Can you guess the future?)
  + Low infrastructure utilization (PEAK LOAD provisioning) 

  + Dedicated infrastructure maintenance team (Can a startup afford it?)

### Silver Lining in the Cloud

+ Rent resources when you want htem, and release them back when they are not needed
  + AKA on-demand resource provisioning
  + AKA elasticity

### Cloud advantages

+ Trade **capital expense** for **variable expense**
+ Benefit from massive **economies of scale**
+ Stop **guessing** capacity
+ Stop spending money running and maintaining data centers
+ **Go global** in minutes

### Google Cloud Platform

+ One of the Top 3 cloud service providers
+ 200+ services provided
+ Reliable, secure and highly performant:
  + Infrastructure that powers 8 services with over 1 billion users: Gmail, Google Search, YouTube
+ Cleanest cloud
  + Net carbon-neutral cloud (electricity used matched 100% with renewable energy)

### Best path to learn GCP

+ Cloud applications make use of multiple GCP services

# Section 02

### Regions and Zones

+ Imagine that your application is deployed in a data center in Londonw
+ possible challenges?
  1. slow access from other parts of the word (**high latency**)
  2. what if the data center crashes? (Low availability)

### Multiple data centers

+ Add one more data center in London
+ Possible challenges
  1. Xxx
  2. Yyy
  3. Zzz

### Multiple regions

+ add a new region: Changhua

+ Possible challenges: 

  1. (partly solved) slow access for users from other parts of the world

     -> Cloud provider to the resecue

  2. (solved) what if one data center crashed

  3. (Solved) what if entire region of London is unavailable?

### Regions

+ Google provides 20+ regions around the world
+ Region: Specific geographical location to hose your resources
+ Advantages:
  + High availability
  + Low Latency
  + Global Footprint
  + Adhere to government regulations

### Zones

+ How to achieve high availability in the same region (or )
+ Each Region has three or more zones
+ (Advantage) Increased availability and fault tolerance within same region

# Section 03

### Google Comput Engine - GCE

+ In corporate data centers, applications are deployed to physical server
+ In the cloud, you rent virtual servers nad deploy applications
+ **Virtual Machines** - Virtual servers in GCP
+ **Google Compute Engine (GCE)** - Provision and Manage Virtual Machines

### Compute Engine - Features

+ Create and manage lifecycle of Virtual Machine (VM) instances
+ **Load balancing** and **auto scaling** for multiple VM instances
+ **Attach storage** (& network storage) to your VM instances
+ Manage **network connectivity and configuration** for your VM instances
+ Goal:
  + Setup VM instances as HTTP(Web) Server
  + Distribute load with Load Balancers

### Compute Engine Hands-on

+ Let's create a few VM instances and play with them
+ Let's check out the lifecycle of VM instances
+ Let's use SSH to connect to VM instances

### Compute Engine Machine Family

> Machine family, machine type

+ What type of hardware do you want to run your workloads on?

+ Different Machine Families for different workloads:

  + **General purpose (E2, N2, N2D, N1)**:

    Best price-performance ratio

    + Web and application servers, Small-medium databases, Dev environments

  + **Memory Optimized (M2, M1)**:

    Ultra high memory workloads

    + Large in-memory databases and In-memory analytics

  + **Compute Optimized (C2)**: 

    Compute intensive workloads

    + Gaming applications

### Compute Engine Machine Types

| Machine name   | vCPUs * | Memory(GB) * | Max number of persistent disks(PDs) | Max total PD size(TB) | Local SSD | Maximum egress bandwidth (Gbps) * |
| -------------- | ------- | ------------ | ----------------------------------- | --------------------- | --------- | --------------------------------- |
| e2-standard-2  | 2       | 8            | 128                                 | 257                   | No        | 4                                 |
| e2-standard-4  | 4       | 16           | 128                                 | 257                   | No        | 8                                 |
| e2-standard-8  | 6       | 32           | 128                                 | 257                   | No        | 16                                |
| e2-standard-16 | 16      | 64           | 128                                 | 257                   | No        | 16                                |
| e2-standard-32 | 32      | 128          | 128                                 | 257                   | No        | 16                                |

+ How much CPU, memory or disk do you want?
  + Variety of machine types are available for each machine family
  + Let's take an example: **e2-standard-2**
    + **e2** : Machine Type Family
    + **standard** : Type of workload
    + **2** : number of CPUs
+ *Memory, disk and networking capabilities increase along with vCPUs

### Image

+ What operating system and what software do you want on the instance?
+ Type of images:
  + Public Images: Provided and maintained by Google or Open source communities or third 

### Compute Engine Hands-on : Setting up a HTTP server

```terminal
#! /bin/bash
sudo su
apt update
apt -y install apache2
sudo service apache2 start
sudo update-rc.d apache2 enable
echo "Hello World" > /var/www/html/index.html
echo "Hello world from $(hostname) $(hostname -i)" > /var/www/html/index.html
```

### Internal and External IP Addresses

+ External (Public) IP addresses are Internet addressable
+ Internal (Private) IP addresses are internal to a corporate network
+ You CANNOT have two resources with the same public (External) IP address
  + However, two different corporate networks CAN have resources with the same Internal (private) IP address
+ All **VM instances** are assigned at least one internal IP address
+ Creation of External IP addresses can be enabled for VM instances
+ DEMO: VM instances - ...

### Static IP Addresses

+ Scenario : How do you get a constant External IP address for a VM instance?
  + Quick and dirty way is to assign an Static IP address to the VM
+ **DEMO** : Using Static IP Address with an VM instance

### Static IP addresses - Remember

+ Static IP **can be switched** to another VM instance in the same project
+ Static IP **remains attached** even if you stop the instance. You have to manually detach it
+ *Remember* : You are **billed for** an Static IP when you are **NOT using it** as well
  + Make sure that you explicitly release an Static IP when you are not using it

> if you are not using a static IP not, it's best that you immediately release it. Or else the billing would be very high.

```terminal
#!/bin/bash
apt update
apt -y install apache2
echo "Hello world from $(hostname) $(hostname -i)" > /var/www/html/index.html
```

### Simplify VM HTTP server setup

+ How do we **reduce** the **number of steps** in creating an VM instance and setting up a HTTP Server?
+ Let's explore a few options:
  + **Startup script**
  + Instance Template
  + Custom Image

### Bootstrapping with Startup script

+ **Bootstrapping**: install OS patches or software when an VM instance is launched
+ In VM, you can configure **Startup script** to bootstrap

### Instance templates

+ Why do you need to specify all the VM instance details (image, instance type etc) every time you launch an instance?
  + How about creating a instance template?
  + Define **machine type**, **image**, **labels**, **startup script** and other properties
+ Used to create **VM instances** and **managed instance groups**
  + Provides a **convenient way** to create similar instances
+ **CANNOT** be updated
  + To make a change, copy an existing template and modify it
+ (Optional) Image family can be specified (example - debian-9):

### Reducing Launch Time with Custom Image

+ Installing OS patches and software at launch of VM instances increase boot up time
+ How about creating a custom image with OS patches and software **pre-installed**?
  + Can be created from an instance, a persistent disk, a snapshot, another image, or a file in Cloud Storage
  + Can be shared across projects
  + (Recommendation) Deprecate old images (& specify replacement image)
  + (Recommendation) Hardening an image - Customize images to your corporate security standards
+ **Prefer** using **Custom Image** to **Startup script**



# Section 04

#### Step00

#### Step02 - create managed instance groups (MIG) V2

##### Instance Groups

+ How do you create a group of VM instances?
  + Instance Group - Group of VM instances managed as a single entity
    + Manage group of similar VMs having similar lifecycle as ONE UNIT
+ Two Types of Instance Groups:
  + Managed: Identical VMs created using a template:
    + Features: Auto scaling, auto healing and managed releases
  + Unmanaged: Different configuration for VMs in same group:
    + Does NOT offer auto scaling, auto healing and other services
    + NOT Recommended unless you need different kinds of VMs
+ Location can be Zonal or Regional
  + Regional gives you higher availability (RECOMMENDED)

##### Managed Instance Groups (MIG)

+ Managed Instance Group 
  + Identical VMs created using an instance template
+ Important Features
  + Maintain certain number of instances
    + If an instance crashes, MIG launches another instance
  + Detect application failures using health checks (Self Healing)
  + Increase and decrease instances based on load (Auto Scaling)
  + Add **Load Balancer** to distribute load
  + Create instances in multiple zones (regional MIGs)
    + Regional MIGs provide higher availability compared to zonal MIGs
  + Release new application versions without downtime
    + Rolling updates: Release new version step by step (gradually). Update a percentage of instances to the new version at a time
    + Canary Deployment: Test new version with a group of instances before releasing it across all instances.

##### Creating MIG (managed instance group)

+ Instance template is mandatory
+ Configure auto-scaling to automatically adjust number of instances based on load:
  + Minimum number of instances
  + Maximum number of instances
  + Autoscaling metrics: CPU Utilization target or Load Balancer Utilization target, or Any other metric from Stack Driver
    + Cool-down period
      + How long to wait before looking at auto scaling metrics again?
      + Scale In Controls: Prevent a sudden drop in no of VM instances
        + Example: Don't scale in by more than 10% or 3 instances in 5 minutes
  + Autohealing: Configure a heath check with initial delay (How long should you wait for your app to intialize before runing a health check?)

#### Step 04 - Updating a Managed Instance Group

+ Rolling update - Gradual update of instances in an instance group to the new instance template
  + Specify new template:
    + (Optional) Specify a template for canary testing
  + Specify how you want the update to be down
    + When should the update happen?
      + Start the update immediately (Proactive) or when instance group is resized later(Opportunistic)
    + How should the update happen?
      + Maximum surge: How many instances are added at any point in time?
      + Maximum unavailable: How many instances can be offline during the update?
+ Rolling Restart/replace: Gradual restart or replace of all instances in the group
  + No change in template BUT replace/restart existing VMs
  + Configure Maximum surge, Maximum unavailable and What you want to do? (Restart/Replace)

#### Step 05: Getting started with cloud load balancing

##### Cloud load balancing

+ Distributes user traffic across instances of any application in single region or multiple regions
  + Fully distributed, software defined managed service
  + Important Features:
    + Health check - Route to healthy instances
      + Recover from failures
    + Auto Scaling
    + Global load balancing with single anycast IP
      + Also supports internal load balancing
+ Enables
  + High Availability
  + Auto Scaling
  + Resiliency

---

#### Step 06: Understanding HTTP, HTTPS, UDP and TCP protocols

```
[Application Layer_layer 7]  --> HTTP HTTPS SMTP

[Transport Layer_layer4] --> TCP TLS UDP

[Network Layer_layer3] --> IP
```



##### HTTP vs HTTPS vs TCP vs TLS vs UDP

+ Computers use protocols to communicate
+ Multiple layers and multiple protocols
+ **Network Layer** - Transfer bits and bytes
+ **Transport Layer** - Are the bits and bytes transferred properly?
+ **Application Layer** - Make REST API calls and Send Emails
+ (Remember) Each layer makes use of the layers beneath it
+ (Remember) Most applications talk at application layer. BUT some applications talk at network layer directly



+ Network Layer:

  + IP (internet protocol): Transfer bytes. Unreliable.

+ Transport Layer:

  + TCP(Transmission Control): Reliability > Performance
  + TLS (Transport Layer Security): Secure TCP
  + UDP (User Datagram Protocol): Performance > Reliability

+ Application Layer:

  + HTTP(Hypertext Transfer Protocol): Stateless Request Response Cycle
  + HTTPS: Secure HTTP
  + SMTP: Email Transfer Protocol
  + and a lot of others...

  

+ Most applications typically communicate at application layer

+ HOWEVER applications needing high performance directly communicate at transport layer:

  + Gaming applications and live video streaming use UDP (sacrifice reliability for performance)

+ Objective: Understand Big Picture. Its OK if you do not understand all details.

##### Cloud Load Balancing - Terminology

+ Backend - Group of endpoints that receive traffic from a Google Cloud load balancer (example: instance groups)
+ Frontend - Specify an IP address, port and protocol. This IP address is the frontend IP for your clients requests.
  + For SSL, a certificate must also be assigned.
+ Host and path rules (For HTTP(S) Load Balancing) - Define rules redirecting the traffic to difference backends:
  + Based on path 
  + Based on Host
  + Based on HTTP headers (Authorization header) and methods
+ Client to Load Balancer: Over internet
  + HTTPS recommended
+ Load Balancer to VM instance: Through Google internal network
  + HTTP is ok. HTTPS is preferred
+ SSL/TLS Termination/Offloading
  + Client to Load Balancer: HTTPS/TLS
  + Load Balancer to VM instance: HTTP/TCP

##### Cloud Load Balancing - Features

| Load Balancer            | Type of Traffic                           | Proxy or pass-through | Destination Ports                    |
| ------------------------ | ----------------------------------------- | --------------------- | ------------------------------------ |
| External HTTP(S)         | Global, External, HTTP or HTTPS           | Proxy                 | HTTP on 80 or 8080<br />HTTPS on 443 |
| Internal HTTP(S)         | Regional, Internal, HTTP or HTTPS         | Proxy                 | HTTP on 80 or 8080<br />HTTPS on 443 |
| SSL Proxy                | Global, External, TCP with SSL offload    | Proxy                 | A big list                           |
| TCP Proxy                | Global, External, TCP without SSL offload | Proxy                 | A big list                           |
| External Network TCP/UDP | Regional, External, TCP or UDP            | pass-through          | any                                  |
| Internal TCP/UDP         | Regional, Internal, TCP or UDP            | pass-through          | any                                  |

##### Load Balancing Across MIGs in Multiple Regions

+ Regional MIG can distribute instances in different zones of a single region
  + Create multiple Regional MIGs in different regions (in the same project)
+ HTTP(S) Load Balancing can distribute load to the multiple MIGs behind a single external IP address
  + User requests are redirected to the nearest region (Low latency)
+ Load balancing sends traffic to healthy instances:
  + If health check fails instances are restarted:
    + (REMEMBER) Ensure that health check from load balancer can reach the instances in an instance group (Firewall rules)
  + If all backends within a region are unhealthy, traffic is distributed to healthy backends in other regions
  + Can contain preemptible instances(先占虛擬機器) as well

##### Quck Review of HTTP(S) Load Balancing Concepts

+ Backend Service - Group of backends or a bucket
+ Backend - A managed instance group
+ URL Maps - Route requests to backend services or backend buckets
  + URL/service-a maps to Backend Service A
  + URL/service-b maps to Backend Service B
+ (Remember) Each Backend Service can have multiple backends in multiple regions

##### HTTP(S) Load Balancing - 1 - Multi Regional Microservice

+ Backend Services
  + One Backend Service for the Microservice
+ Backends
  + Multiple backends for each microservice MIG in each region
+ URL Maps
  + URL/service-a maps to the microservice Backend Service
+ Global routing: Route user to nearest instance (nearest regional MIG)
  + Needs Networking Premium Tier
    + As in STANDARD Tier:
      + Forwarding rule and its external IP address are regional
      + All backends for a backend service must be in the same region as the forwarding rule
+ Backend Service:
  + One Backend Service for each the Microservice
+ Backends:
  + Each microservice can have multiple backend MIGs in different regions
  + In the example, we see one backend per microservice
+ URL Maps:
  + URL / service-a -> Microservice A Backend Service
  + URL /service-b -> Microservice B Backend Service
+ Backend Services
  + A Backend Service for each Microservice version
+ Backends
  + Each microervice version can have multiple backend MIG in different regions
    + In the example, we see one backend per microservice version
+ URL Maps
  + /service-a/v1 -> Microservice A V1 Backend Service
  + /service-a/v2 -> Microservice A V2 Backend Service
  + /service-b -> Microservice B Backend Service
+ Flexible Architecture: With HTTP(S) load balancing, route global requests across multiple versions of multiple microservices!

# Section 05



# Section 06



# Section 07





# Section 08



# Section 09





# Section 10 Google Cloud Functions



# Section 11



# Section 12



# Section 13 



# Section 14





# Section 15

> Exploring Object Storage in Google Cloud Platform - Cloud Storage

### Cloud Storage

+ Most popular, very flexible & inexpensive storage service

  + Serverless: Autoscaling and infinite scale

+ Store large objects using a **key-value** approach:

  + Treats entire object as a unit (Partial updates not allowed)
    + Recommended when you operate on entire object most of the time
    + Access Control at Object level
  + Also called **Object Storage**

+ Provides REST API to access and modify objects

  + Also provides CLI (gsutil) & Client Libraries (C++, C#, Java, Node.js, PHP, Python & Ruby)

    > `gsutil`, not `gcloud`

+ Stores all file types - text, binary, backup & archives

  +  Media files and archives, Application packages and logs
  + Backups of your databases 
  + Staging data during on-premise to cloud database migration

### Objects and Buckets

- Objects are stored in buckets
  - Bucket names are **globally unique**
  - Bucket names are used as part of object URLs 
    - Naming rule: 只能有小寫字母、數字、-、_、. (hyphen, underscore, period)
  - 3 ~ 63 characters max. Can't start with **goog** prefix or should not contain **google** (even misspelled)
  - Unlimited objects in a bucket
  - Each bucket is associated with a project
  - Each object is identified by an **unique key**
    - Key is unique in a bucket
  - Max object size is **5 TB** (single object)
    - but you can store unlimited number of such objects

### Storage Class - Introduction

+ Different kinds of data can be store in Cloud Storage
  + Media files, archives
  + App packages and logs
  + Backups of your databases or storage devices
  + Long term archives
+ Huge variation in **access patterns**
+ If accessed less frequently, can the storaged object be paid at a cheaper price?
  + **Storage classes** help to optimize your costs based on your access needs
    + Design for durability of 99.999999999% (11 9's)

### Storage Classes - Comparision

| Storage Class    | Name     | Min storage duration | Typical monthly availability                     | Use case                               |
| ---------------- | -------- | -------------------- | ------------------------------------------------ | -------------------------------------- |
| Standard         | STANDARD | None                 | Multi/dual region: > 99.99%<br />Regions: 99.99% | Frequently used data                   |
| Nearline storage | NEARLINE | 30 days              | Multi/dual region: 99.95%<br />Regions: 99.99%   | Read or modify once a month on average |
| Coldline storage | COLDLINE | 90 days              | Multi/dual region: 99.95%<br />Regions: 99.99%   | Read or modify at most  once a quarter |
| Archive storage  | ARCHIVE  | 365 days             | Multi/dual region: 99.95%<br />Regions: 99.99%   | Less less once a year                  |

### Features across Storage Classes

+ High durability (99.999999999% annual durability)
+ Low latency (first byte typically in tens of milliseconds)
+ Unlimited storage
  + Autoscaling (No configuration needed)
  + NO minimum object size
+ Same APIs across storage classes
+ **Committed SLA** is 99.95% for multi region and 
  99.9% for single region for Standard, Nearline, Coldline storage classes
  + No committed SLA for archive storage

> Committed SLA 服務等級協定/服務級別協定

### Ｏbject Versioning

+ Prevents **accidental deletion** and provides history
  + Enabled at bucket level
    + Can be turned on/off at any time
  + Live version is the latest version
    + If you delete live object, it becomes noncurrent object version
    + If you delete noncurrent object version, 
  + Older versions are uniquely identified by (obejct key + a generation number)
  + 

### Object Lifecycle Management

+ Files are frequently accessed when they are created
  + Generally **usage reduces with time**
  + How do you save costs by **moving files automatically between storage classes**?
    + Solution: Object Lifecycle Management
+ Identify objects using conditions based on:
  + Age, CreatedBefore, IsLive, MatchesStorageClass, NumberOfNewerVersions etc
  + Set multiple conditions: all conditions must be satisfied for action to happen
+ Two kinds of actions:
  + **SetStorageClass** actions (change from one storage class to another)
  + **Deletion** actions (delete objects)
+ Allowed Transitions:
  + (Standard or Multi-Regional or Regional) to (Nearline or Coldline or Archive)
  + Nearline to (Coldline or Archive)
  + Coldline to Archive	

### Encription

+ (Default) Cloud storage encrypts data on the server side.
+ **Server-side** encryption: Performed by GCS after it receives data
  1. Google-managed - default (No configuration needed)
  2. Customer-managed - keys managed by customer in **Cloud KMS**
     + GCS service account should have access to customer-managed keys in KMS to be able to encrypt and decrypt files
  3. Customer-supplied - customer supplies the keys with every GCS operation
     + Cloud storage does NOT store the key
     + Customer is responsible for storing and using it when making API calls
       + Use API headers when making API calls
         + x-goog-encrytion-algorithm, 
           x-goog-encryption-key (Base64 encryption key), 
           x-goog-encryption-key-sha256 (encryption key hash)
       + OR when using gsutil: In both configuration file, configure encryption_key under GSUtil section
+ (Optional) **Client-side** encryption - encryption performed by customer before upload
  + GCP does NOT know about the keys used
  + GCP is NOT involved in encryption or decryption

### Unstanding Cloud Storage Metadata

+ Each object in Cloud Storage can have **Metadata** associated with it
  + Key value pairs ex: `storageClass:STANDARD`
  + **Fixed-key metadata**: Fixed key - Changing value
    + `Cache-Control: public, max-age=3600` 
      (Is caching allowed? If so, for how long?)
    + `Content-Disposition: attachment; filename="myfile.pdf"`
      (Should content be displayed inline in the browser or should it be an attachment, which can be downloaded)
    + `Content-Type: application/pdf` 
      (What kind of content does this object have?)
    + etc...
  + **Custom metadata**: You can define your own keys and values
  + **Non-editable metadata**: You cannot edit these directly
    + Storage class of the object, customer-managed encryption keys etc.

### Cloud Storage Bucket Lock - Meet Compliance Needs

+ How do you ensure that you **comply with regulatory and compliance requirements** around immutable storage in a Cloud Storage bucket?
+ Configure **data retention policy** with retention period:（數據保存策略)
  + How long should objects in the bucket be retained for?
    + "Objects in the bucket can only be deleted or replaced once their age is greater than the retention period"
  + You can set it **while creating a bucket or at a later point in time**
    + Applies automatically to existing objects in the bucket (as well as new objects added in)
  + Once a retention policy is locked:
    + You **CANNOT** remove retention policy or reduce retention period (but you can increase retention period)
    + You **CANNOT** delete the bucket unless all objects in bucket have age greater than retention period

### Transferring data from on premises to cloud

+ Most popular data destination is **Google Cloud Storage**
+ Options:
  1. **Online Transfer**: Use `gsutil` or API to transfer data to GCS
     + Good for one time transfers
  2. **Storage Transfer Service**: Recommended for large-scale (petabytes) online data transfers from your private data centers, AWS, Azure, and Google Cloud
     + You can set up a repeating schedule
     + Supports incremental transfer (only transfer changed objects)
     + Reliable and fault tolerant - continues from where it left off in case of errors
  3. Storage Transfer Service vs gsutil:
     + `gsutil` is recommended only when you are transferring less than 1TB from on-premises or another GCS bucket
     + `Storage Transfer Service` is recommended if either -
       1) Transferring more than 1TB from anywhere, or
       2) Transferring from another cloud

### Migrating Data with Transfer Appliance

+ Transfer Appliance: 
  + Copy ship and upload data to GCS
  + Recommended if your data size is greater than **20TB**, or online **transfer time takes > 1 week**
  + Process:
    1. Request an appliance
    1. Upload your data
    1. Ship the appliance back
    1. Google uploads the data
  + Fast copy (upto 40 Gbps)
  + AES256 encryption - Customer-managed encryption keys
  + Order **multiple devices** (TA40, TA300) if need

### Understanding Cloud Storage Best Practices

+ Avoid use of sensitive info in bucket or object names
+ Store data in the **closest region** (to your users)
+ Ramp up **request rate gradually**
  + No problems upto 1000 write request per second or 5000 read requests per second
  + BUT beyond that, take at least 20 minutes to double request rates
+ Use **Exponential backoff** if you receive 5xx (server error) or 429 (to many requests) error
  + Retry after 1, 2, 4, 8, 16, ...seconds
+ **Do NOT use sequential** numbers or timestamp as object keys
  + Recommended to use completely random object names
  + Recommended to add a hash value before the sequence number or timestamp
+ **Use Cloud Storage FUSE** to enable file system access to Cloud Storage
  + Mount Cloud Storage buckets as file systems on Linux or macOS systems

### Cloud Storage - command line

+ `gsutil` is the CLI for Cloud Storage (NOT `gcloud`)

+ Cloud Storage

  + `gsutil mb gs://BKT_NAME` -> Create Cloud Storage bucket 

  + `gsutil ls -a gs://BKT_NAME` -> List current and non-current object versions

    > `-a` for displaying non-current ones

  + `gsutil cp gs://SOURCE_BKT/SRC_OBJ gs://DESTINATION_BKT/NAM_COPY` -> Copy objects

    + `-o 'GSUtil:encryption_key=ENCRYPTION_KEY'` -> Encrypt object

  + `gsutil mv` (Rename or move objects)

    + `gsutil mv gs://BKT_NAME/OLD_OBJ_NAME gs://BKT_NAME/NEW_OBJ_NAME`
    + `gsutil mv gs://OLD_BUCKET_NAME/OLD_OBJ_NAME gs://NEW_BKT_NAME/NEW_OBJ_NAME`

  + `gsutil rewrite -s STORAGE_CLASS gs://BKT_NAME/OBJ_PATH` (e.g. Change Storage Class for objects)

  + `gsutil cp`: Upload and Download Objects

    + `gsutil cp LOCAL_LOCATION gs://DESTINATION_BKT_NAME/` -> Upload
    + `gsutil cp gs://BKT_NAME/OBJ_PATH LOCAL_LOCATION` -> Download

  + `gsutil versioning set on/off gs://BKT_NAME` -> Enable/Disable versioning

  + `gsutil uniformbucketlevelaccess set on/off gs://BKT_NAME`
     -> If set off, you can configure permissions to objects at the level of individual objects

  + `gsutil acl ch` -> Set access permissions for specific objects
     -> Used when uniform-bucket-level-access is set OFF. 

    > `acl` - access control list
    >
    > `ch` - change
    >
    > E.g.: `gsutil acl ch -u AllUsers:R gs://BKT_NAME/OBJ_PATH` 
    >           -> make specific object public
    >
    > E.g.: `gsutil acl ch -u john.doe@gmail.com:WRITE gs://BKT_NAME/OBJ_PATH`
    >           -> **Permissions** - READ(R), WRITE(W), OWNER(O)
    >           -> **Scope** - User, allAuthenticatedUsers, allUsers(-u), Group(-g), Project(-p) etc
    >
    > E.g.: `gsutil acl set JSON_FILE gs://BKT_NAME`
    >           -> use Json files to set ACLs on the bucket

  + `gsutil iam ch MBR_TYPE:MBR_NAME:IAM_ROLE gs://BKT_NAME`
     -> set up IAM role.

    > {memberType}:{memberName}:{IAMrole}
    > E.g.: `gsutil iam ch user:melissa@gmail.com:objectCreator gs://BKT_NAME`
    > E.g.: `gsutil iam ch allUsers:objectViewer gs://BKT_NAME`
    >         -> make the entire bucket readable to public

  + [Temporary] `gsutil signal -d 10m YOUR_KEY gs://BKT_NAME/OBJ_PATH`
    -> signed URL for temporary access ( -d {10_minutes} {service_account_key})

### Scenarios

| Scenario                                                     | Solution                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| I will frequently access objects in a bucket for 30 days. After that I don't expect to access objects at all. We have compliance requirements to store objects for 4 years. How can I minimize my costs? | Initial Storage Class - Standard.  Lifecycle policy: Move to Archive class after 30 days. Delete after years. |
| I want to transfer 2 TB of data from Azure Storage to Cloud Storage | Use Cloud Storage Transfer Service                           |
| I want to transfer 40TB of data from on premises to Cloud Storage | Use Transfer Appliance                                       |
| Customer wants to manage their Keys                          | **Customer-managed**: Keys managed by customer in **Cloud KMS** |
| Regulatory compliance: Object should not modified for 2 years | Configure and lock **data retention policy**                 |

# Section 16 - 

>Identity and access management - IAM



# Section 17 



# Section 18





Section 19



Section 20



# Section 21

+ To operate cloud app effectively
  + Is my app healthy?
  + Are the users experiencing any issues?
  + Does my db have enough space?
  + Are my servers running in an optimum capacity?
+ **Cloud Monitoring** - infrastructure monitoring





### Cloud Debugger



### Cloud Proflier

+ How do you identify performance bottlenecks in production?
+ Cloud Profiler - Statistical, low-overhead profiler
  + Continuously gathers CPU and Memory usage from production



### Error Reporting

+ How do you idenfiy production problems in real time?
+ Real-time exception monitoring:
  + Aggregates and displays error reported from cloud services (using stack traces)
  + Centralized Error Management console:
    + Identity and manage top errors or recent errors
  + Use Firebase Crash Reporting for errors from Android and iOS client applications
  + Supported for GO, Java, .NET, Node.js, PHP, Python and Ruby
+  
+  

### Cloud Logging, Monitoring .. - Stackdriver

| Stackdriver Service | New Service Name |
| ------------------- | ---------------- |
|                     |                  |

> old names - new names: these are the same

### Cloud Operations Scenarios

| Se                                                           | So                                               |
| ------------------------------------------------------------ | ------------------------------------------------ |
| To record all operations/requests on all objects in a bucket (for auditing) | Turn on data access audit logging for the bucket |
| To trace a request across multiple microservices             | Cloud Trace                                      |
| To identify prominent exceptions (or errors) for a specific microservice | Error Reporting                                  |
| To debug a problem in production by executing step by step   | Cloud Debugger                                   |
| To look at the logs for a specific request                   | Cloud Logging                                    |
​                                                  

# Section 22

### 

+ Well defined hierarchy:
  + 
+ Resources are created in projects
+ A folder can contain multiple projects
+ Organization can contain multiple folders

### Resource Hierarchy - Recommendations for Enterprises

+ Create separate projects for different environments
  + Complete isolation between test and production environments
+ Create separate folders for each department
  + Isolate production applications of one department from another
  + We can create a shared folder
+ One project per application per environment
  + 



### Billing Accounts

+ Billing Account is mandatory for creating resources in a project:
  + 
+ Billing Accounts can be associated with one or more projects
+ You can have multiple billing accounts in a Organization
+ (Recommendation)
+ Two Types
  + Self Serve
  + Invoiced

### Managing Billing - Budget, Alerts and Exports

+ Setup a Cloud Billing Budget to avoid surprises:
  + ()
  + Default alert thresholds
+ Billing data can be exported (on a schedule) to:
  + BigQuery
  + Cloud Storage ()

### IAM Best Practices

+ **Principle of Least Privilege** - Give least possible privilege needed for a role
  + Basic Roles are NOT recommended
    + Prefer predefined roles when possible
  + Use Service Accounts with minimum privileges
    + Use different Service Accounts for different apps/purposes
+ **Separation of Duties** - Involve at least two people in sensitive tasks
  + Example: Have separate deployer and traffic migrator roles
    + AppEngine provides App Engine
+ Constant Monitoring:
  + Archive Cloud Audit Logs in Cloud Storage buckets for long term retention
+ Use Groups when possible
  + Makes it easy to manage users

### User Identity Management in Google Cloud

+ Email used to create free trial account => "Super Admin"
  + 
+ 
+ However ...
+ Option1:
+ Option2:
  + Federate Google Cloud with your identity provider

> Google Workspace (formerly G Suite)

### Corporate Directory Federation

+ Federate Cloud Identity or Google Workspace with your external identity provider (IdP) such as Active Directory of Azure Active Directory
+ Enable Single Sign On:
  1. Users are redirected to an external IdP to authenticate
  2. When users are authenticated, SAML assertion is sent to Google sign-in
+ Examples
  + Federate Active Directory with Cloud Identity by using Google Cloud Directory Sync (GCDS) and Active Directory Federation Services (AD FS)
  + Federating Azure

### IAM Members/Identities

+ Google Account - Represents a person (an)
+ Service account - Represents an application account (Not person)
+ Google group - Collection - Google and Service Accounts
  + 
+ Google Workspace domain: Google Workspace (formerly G suite) provides collaboration services for enterprises:
  + 
+ 

### IAM Members/Identities - Use Cases

| Sc                                                           | Sol                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| All members in your team have G Suite accounts. You are creating a new production project and would want to provide access to your operations team. | Create a Group with all your operations team. Provide access to production project to the Group. |
| All members in your team have G Suite accounts. You are setting up a new project. You want to provide a one time quick access to a team member. | Assign the necessary role directory to G Suite email address of your team member. If it's not a one time quick access, the recommended approach would be to create a Group. |
| You want to provide an external auditor access to view all resources in your project BUT he should NOT be able to make any changes. | Give them roles/viewer role (Generally basic roles are NOT recommended BUT it is the simplest way to provide view only access to all resources!) |
| Your application deployed on a GCE VM (Project A) needs to access cloud storage bucket from a different project (Project B) | In Project B, assign the right role ...                      |



### Organization Policy Service

+ How to enable **centralized constraints** on all resources created in an Organization?
  + Configure Organization Policy
  + e.g.: Disable creation of Service Accounts
  + e.g.: Allow/Deny creation of resources in specific regions
+ Needs a Role - Organization Policy Administrator
+ (Remember) IAM focuses on Who
  + Who can take specific actions on resources?
+ (Remember) Organization Policy focuses on What
  + What can be done on specific resources?

### Resource Hierarchy & IAM Policy

+ IAM Policy can be set at any level of the hierarchy
+ Resources inherit the policies of All parents
+ The effective policy for a resource is the union of the policy on that resource and its parents
+ Policy inheritance is transitive
  + 
+ You cannot restrict



### Cloud BigQuery Roles

> Data Operations: View 
>
> Query Operations:

+ Cloud BigQuery IAM Roles
  + BigQuery Admin
  + BigQuery Data Owner
  + BigQuery Data Editor
  + BigQuery Data Viewer
  + BigQuery Job User
  + BigQuery User
+ To see data, 
+ BigQUery DO or Data Viewer roles do NOT have access to jobs



### Corporate Directory Federation

+ Federate Cloud Identity or Google Workspace with your external identity provider (IdP) such as Active Directory or Azure AD
+ Enable Single Sign On:
  1. Users are redirected to an external IdP to authenticate
  2. When users are authenticated, SAML assertion is sent to Google Sign-In
+ Examples
  + Use Identity as a service (IDaaS) such as Okta as IdP (identity provider)
    + Use IDaaS (like Okta) for single sign-on
  + Use Active Directory as IdP
    + Use Active Directory Federation Services (AD FS) for single sign-on
    + Use Google
  + Use Azure AD as IdP
    + Use Azure AD for single sign-on

### IAM - Scenarios

| Sc                                                           | Des                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| To give permanent access to a sub set of objects in a Cloud Storage bucket | Use ACLs                                                     |
| To give permanent access to the entire bucket in a Cloud Storage bucket | Use IAM                                                      |
| To provide time limited access to a specific obj in a Cloud Storage bucket | Create a Signed URL                                          |
| To give access to a set of resources to your development team | Create a group with your development team as member. Bind the right predefined roles to your group. |
| Which role? Upload obj to Cloud Storage                      | Storage Object Creator                                       |
| Which role? Manage K8S API objects                           | Kubernetes Engine Developer                                  |
|                                                              |                                                              |
|                                                              |                                                              |
|                                                              |                                                              |



Section 23

221



Section 24

### Ｎeed for Asynchronous Communication



### Asynchronous Communication - Decoupled

+ Create a topic and have your applications put log messages on the topic
+ Logging service picks them up for processing when ready
+ Advantages
  + Decoupling: Publishers (apps) don't care
  + Availability:
  + Scalability:
  + Durability:



### Pub/Sub

+ Reliable, scalable, fully-managed asynchronous messaging service
+ Backbone for **highly available** and **highly scalable** solutions
  + Auto scale to process billions of messages per day
  + 
+ Usecases: Event ingestion and delivery for streaming analytics pipelines
+ Supports push and pull message deliveries

### Pub/Sub - How does it work?

+ **Publisher** - Sender of a message
  + Publisher send messages by making HTTPS requests to pubsub.googleapis.com
+ **Subscriber** - Receiver of the message
  + **Pull** - Subscriber pulls messages when ready
    + Subscriber makes HTTPs requests to pubs.googleapis.com
  + **Push** - Messages are snet to subscribers
    + Subscribers provide a web hook endpoint at the time of registration
    + When a message is received on the topic, a HTTPS POST request is sent to the web hook endpoints
+ **Very Flexible**:
  + Publisher(s) and Subscriber(s) relationships: one to many, many to one, many to many

### Pub/Sub - Getting ready with Topic and Subscriptions

1. create topic
2. create subscription
   + subscribers register to the topic
   + each subscription represents discrete pull of messages form a topic
     + 

### Pub/Sub - Sending and Receiving a message

+ Publisher sends a message to topic
+ Message **individually** deliver to each and every subscription
+ Subscribers send acknowledgements(s)
+ Messages(s) are removed

> ```terminal
> gcloud config set project glowing-gracias-304608
> gcloud pubsub topics create topic-from-gcloud
> gcloud pubsub subscriptions create subscription-gcloud-1 --topic=topic-from-gcloud
> gcloud pubsub subscriptions create subscription-gcloud-2 --topic=topic-from-gcloud
> gcloud pubsub subscriptions pull subscription-gcloud-2
> gcloud pubsub subscriptions pull subscription-gcloud-1
> gcloud pubsub topics publish topic-from-gcloud --message="My First Message"
> gcloud pubsub topics publish topic-from-gcloud --message="My Second Message"
> gcloud pubsub topics publish topic-from-gcloud --message="My Third Message"
> ```
>
> 

# Section 25

> Hybrid Cloud options

### Cloud VPN

+ Cloud VPN - Connect on-premise to GCP network over internet
  + Implemented using **IPSec VPN Tunnel**
  + Traffic through internet (public)
  + Traffic encrypted using **Internet Key Exchange** protocol
+ Two types of Cloud VPN solutions:
  + **HA VPN** (SLA of 99.99% service availability with two external IP address)
    + Only dynamic routing (BGP) supported
  + **Classic VPN** (SLA of 99.9% service availability, a single external IP address)
    + Supports Static routing (policy-based, route-based) and dynamic routing using BGP
+ Easy to establish: Does NOT need carrier circuits or contracts
+ Go for Cloud VPN if:
  + You want the network to encrypt traffic OR
  + You want a lower throughput, low cost solution OR
  + 
+ 

### Cloud VPN - VPN Gateway, peer gateway and cloud router

1. HA VPN (**high availability VPN**)
   + High availability (99.99 SLA, within region)
   + Needs a cloud HA VPN gateway
     + Regional resources within two interfaces
     + Connects to an on-premises VPN gateway (or peer gateway) through VPN tunnels
2. **Classic VPN**
   1. No high availability
   2. Needs a Google Compute Engine VPN gateway

(REMEMBER) VPN gateway - regional resource

(REMEMBER) Cloud router enables dynamic routing: Enables automatic route update when network topology changes

### Cloud Interconnect

+ High speed, highly available, low-latency private connection into Google Cloud from your company's on-premises network
+ **Dedicate Interconnect**: Ideal if you need high-bandwidth connection for large data transfers
  + Min private connection speed of 10Gbps (OPTIONS: 10 GBps or 100 Gbps)
    + Go upto 8 x 10 Gbps (80Gbps) circuits, or 2 x 100 Gbps(200 Gbps) circuits for each connection
  + Takes time to establish
+ **Partner Interconnect**: Ideal if you need a private connection with lower bandwidth needs
  + 50Mbps to 10Gbps

+ Data exchange happens through a private network:
  + Communicate using VPC network's internal IP addresses from on-premise network
  + Reduces egress costs
    + As public internet is not used

### Hybrid Connectivity - Remember

+ When you connect networks, ensure that resources on the networks **use different range of IP address**!
+ Always think: What will we do if things go wrong?
  + Have a fallback option if the primary connection from on-premise to GCP fails
    + Dedicated interconnect as primary
    + VPN as backup in case of failure
+ Remember that there is a third hybrid connectivity option:
  + Direct Peering: Connect customer network to google network using network peering
    + Direct path from on-ppremises network to Google services
    + Not a GCP Service
      + Lower level network connection outside of GCP
    + NOT RECOMMENDED:
      + Use Cloud Interconnect and Cloud VPN

# Section 26

### BigQuery - Datawarehouse

+ Exabyte scale modern Datawarehousing solution from GCP
  + Relational database (SQL, schema, consistency etc)
    + Use **SQL-like commands** to query massive datasets
  + Traditional (Storage + Compute) + Modern (Realtime + Serverless)
+ When we are talking about a Datawarehouse, **importing and exporting data (and formate) becomes very important**
  + Load data form a variety of sources, incl. streaming data
    + Variety of import formats - CSV/JSON/Avro/Parquet/ORCDatastore backup
  + Export to Cloud Storage (long term storage) & Data Studio (visualization)
    + Formats - CSV/JSON (with Gzip compression), Avro (with deflate or snappy compression)
  + Automatically expire data (Configurable Table Expiration)
  + Query external data sources without storing data in BigQuery
    + Cloud Storage, Cloud SQL, BigTable, Google Drive
    + Use **Permanent or Temporary** external tables
+ Automatically expire data (**Configurable Table Expiration**)
+ Query **external data sources** without storing data in BigQuery
  + Cloud Storage, Cloud SQL, BigTable, Google Drive
  + Use **Permanent** or **Temporary** external tables

### BigQuery - Accessing and Querying Data

+ Access databases using
  + Cloud console
  + Bq command-line tool (NOT gcloud)
  + BigQuery Rest API, or
  + HBase API based libraries (Java, .NET, Python)
+ (Remember) BigQuery queries can be expensive as you are running them on large data sets
+ (Best practice) **Estimate BigQuery queries before running**
  1. Use UI(console) / bq (--dry-run) : Get scanned data volume (estimated)
  1. Use Pricing Calculator : Find price for scanning 1MB data. Calculate cost.

### Partitioning and Clustering BigQuery Tables - Use Case

+ You pay for BigQuery queries by the amount of data scanned
+ How do you reduce your costs of querying BigQuery and improve performance?
+ Scenario: Imagine a Questions table with millions of rows
  + You want to find all questions asked between a date range (date between 2022-10-02 and 2028-10-02) belonging to a specific category
    + If you have a single questions table you need to scan all the rows
      + Partitioning - Divide table into multiple segments (e.g. by date)
      + Clustering - Group related data (e.g. by category)

### Partitioning and Clustering BigQuery Tables

+ **Partitioning**: Table is divided into segments
  + Makes it easy to manage and query your data
  + Improves performance and reduces costs
  + Partition based on Ingestion time (arrival time) OR a column (TIMESTAMP, DATE, or DATETIME, or INTEGER)
  + (DEFAULT) All partitions will share same schema as table
  + Allows you to expire (delete) parts of table data easily (portion_expiration_days)
+ **Clustering**: Organize table data based on the contents of one or more columns
  + Goal: colocate related data and eliminate scans of unnecessary data
  + Avoid creating too many small partitions (of less than 1GB). In those cases, prefer Clustering

### Partitioning and Clustering BigQuery Tables - Syntax

```SQL
CREATE TABLE my_data_set.questions_partitioned_and_clustered
...
...
PARTITIONED BY
    DATE(created_date)
    CLUSTER BY category
...
OPTIONS (
    expiration_timestamp=TIMESTAMP "2023-05-01 03:02:00 UTC",
    partition_expiration_days=7
)
```



### Expiring Data in BigQuery

```sql
CREATE SCHEMA mydataset
OPTIONS(
  default_table_expiration_days=3.75  
  )
  
A LTER TABLE mydataset.mytable
SET OPTIONS (
  expiration_timestamp=TIMESTAMP "2025-01-01 00:00:00 UTC",
  partition_expiration_days=7
)
```

+ You pay for data stored in BigQuery:
  + How can you automatically delete (expire) data which is not needed?
+ Big Query Hierarchy: Data Set > Table > Partitions
  + You can configure expiration at each level
    + Configure default table expiration (default_table_expiration_days) for datasets
    + Configure expiration time (expiration_timestamp) for tables
    + Configure partition expiration (partition_expiration_days) for partitioned tables

+ Best Practice: Expire Tables and Partitions you are NOT using!

### Importing Data into BigQuery

+ **Batch Import** (FREE):
  + Import from Cloud Storage and local files
  + Import after processing by Cloud Dataflow and Cloud Dataproc
+ **Streaming Import** ($$$$):
  + From Cloud Pub/Sub, Streaming Inserts
  + Import after processing by Cloud Dataflow and Cloud Dataproc
+ **Federated Queries**: Query external data
  + Cloud Storage, Cloud SQL, BigTable, Google Drive
+ **BigQuery Data Transfer Service**:  Import from
  + Google SaaS apps (Google Ads, Cloud Storage, etc)
  + External cloud storage providers, e.g. Amazon S3
  + Data warehouses - Teradata, Amazon Redshift

### Streaming Data into BigQuery

+ Loading data in bulk is free **but streaming data is NOT FREE**
  + AND there are a lot of limitations (Use with caution!)
+ Streaming data can contain duplicates. How can you avoid duplicates?
  + Add **insertId** with each streaming insert:
    + `insertId` is used to provide best effort de-duplication (for up to one minute)
      + For strict de-duplication and transactions, try Google Cloud Datastore
+ There are **strict streaming quotas** with BigQuery
  + IF you are NOT populating `insertId`
    + Max bytes per second - 1GB per sec, per project 
      (REMEMBER per project - NOT per table)
  + ELSE (i.e. you are using `insertId`)
    + Maximum rows per second per project
      + US and EU multi-regions: 500,000 
        Other locations: 100,000
      + Per table limitation: 100,000
    + Maximum bytes per second: 100 MB
  + If you have streaming of millions of rows per second, prefer BigTable!

### Understanding BigQuery Best Practices

+ Estimate your queries before running them
  + `bq --dry_run flag` or dryRun API parameter
+ Use clustering and partitioning for your tables
+ **Avoid streaming inserts** when possible
  + Loading data in bulk is free but streaming data is NOT FREE
  + Offers Best effort de-duplication (when you use `insertId`)
  + Remember Quota limits
+ **Expire Data Automatically**
  + Configure default table expiration (default_table_expiration_days) for datasets
  + Configure expiration time for tables
  + Configure partition expiration for partitioned tables
+ Consider **Long-term storage** option
  + Long-term storage: table in which data is NOT edited for 90 consecutive days
  + Low Storage cost - similar to Cloud Storage Nearline
+ BigQuery is fast for **complex queries**
  + BUT it is not as well optimized for narrow-range queries (prefer Cloud Bigtables)
  + (REMEMBER) Too much complexity in setting up a query
+ **Optimize BigQuery usage** using audit logs:
  + Analyze queries/jobs that were run earlier
  + Stream your audit logs (BigQueriesAuditMetadata) to BigQuery
+  

### Cloud Dataproc

+ Managed **Spark and Hadoop** service:
  + Variety of jobs are supported
    + Spark, PySpark, SparkR, Hive, SparkSQL, Pig, Hadoop
  + Perform complex batch processing
+ **Multiple Cluster Modes**:
  + Single Node / Standard / High Availability (3 masters)
  +  Use regular / preemptible VMs
+ Use case: Move your Hadoop and Spark clusters to the cloud
  + Perform your machine learning and AI development using open source frameworks
+ (REMEMBER) Cloud Dataproc is a data analysis platform
  + You can export cluster configuration but NOT data
+ (ALTERNATIVE) BigQuery - When you run SQL queries on Petabytes
  + Go for Cloud Dataproc when you need more than queries
     (e.g. Complex batch processing Machine Learning and AI workloads)

# Section 27

### Data Lifecycle

+ Four Steps:
  1. Ingest: Stream or batch ingest
  2. Store: Durably and cost efficiently store data in a convenient format
  3. Process and analysis: Convert data to information (normalization or aggregations)
  4. Explore and visualize: Flexibility to play with data/information. Get and share insights

### Data Lifecycle - 1 - Ingest

+ Streaming: Pub/Sub
+ Batch: Storage Transfer Service, BigQuery Transfer Service, Transfer Appliance, gsutil etc
+ Database migration: Migrate data from other sources to Google Cloud
  + Data Migration Service (Simplifying migrations to Cloud SQL)
  + Batch transfer to Cloud Storage
  + Load data into database from Cloud Storage using Dataflow

### Data Lifecycle - 2 - Store

| Service         | Solution                                                     |
| --------------- | ------------------------------------------------------------ |
| Cloud Storage   | Object Storage (unstructured data)                           |
| Cloud SQL       | Managed MySQL, PostgreSQL, and MS SQL Server databases<br />Relational, pre-defined schema, strong transactions, regional |
| Cloud Spanner   | Horizontally scalable relational database<br />Relational, pre-defined schema, strong transactions, high availability, and global scale |
| Cloud Firestore | Flexible, scalable, transactional NoSQL database             |
| Cloud Bigtable  | Managed wide-column NoSQL.  <br />Petabyte scale, Real-time apps and large-scale analytical time-series workloads, single-row transactions |
| BigQuery        | Managed data warehouse                                       |
| Custom Database | Use cloud marketplace to deploy an open source database of your choice - MongoDB, Cassandra, etc. |

### Data Lifecycle - 3 - Process and analysis	

> Row Data > Actionable Information (Clean, Transform)

| Service                    | Solution                                                     |
| -------------------------- | ------------------------------------------------------------ |
| Dataprep                   | Clean and prepare data.  Fully managed, No-Ops.<br />Usecases: Clean data on-boarded from external sources. Prepare data for ML.<br />Visual approach for non programmers |
| Cloud Data Loss Prevention | Scan, discover, classify, and report on data in Cloud Storage, BigQuery, and Datastore (mask, tokenize, and transform sensitive elements) |
| Dataflow                   | More flexible ETL pipelines (Fully managed, No-Ops, Batch and Streaming) |
| Dataproc                   | Complex processing using Spark and Hadoop. <br />Needs a cluster with compute engine VMs. <br />Usecases: Machine learning, migrate existing Spark and Hadoop workloads |

### Data Lifecycle - 4 - Explore and visualize

| Service               | Solution                                                     |
| --------------------- | ------------------------------------------------------------ |
| Cloud BigQuery        | Managed data warehouse, Standard SQL,<br /> serverless, separate storage and compute |
| ML - pre built models | Vision API, Speech-to-Text, Natural Language API, Video Intelligence API etc |
| ML - custom models    | Use AI platform (based on TensorFlow)<br />Use Dataflow for pre-processing |
| Cloud Datalab         | Web based tool to explore, analyze and visualize data. <br />Based on Jupyter notebooks (Use Python, SQL queries etc).  <br />Support for popular data-science toolkits - pandas, numpy and scikit-learn |
| Cloud Data Studio     | Dashboarding and visualization<br />Live charts and graphs based on data in Cloud SQL, BigQuery etc |
| Cloud Data Catalog    | Data discovery and metadata management<br />Unified view of all datasets<br />Tag sensitive data using Cloud Data Loss Prevention (DLP) |



### Big Data and Analytics in GCP

| Service            | Solution                                                     |
| ------------------ | ------------------------------------------------------------ |
| Pub/Sub            | Foundation for stream analytics and event-driven systems     |
| BigQuery           | Serverless data warehouse to analyze petabytes of data.  Scale storage and compute separately. |
| Google Data Studio | Managed visual analytics service                             |
| Dataflow           | Data pipelines for (Stream + Batch) use cases                |
| Dataproc           | Managed Apache Spark and Apache Hadoop clusters              |
| Dataprep           | Clean and prepare data (structured and unstructured)         |
| Datalab            | Explore, analyze and visualize data on Jupyter notebooks (use python, SQL queries etc).  <br />Integrates well with BigQuery |
| Cloud Composer     | Managed workflow orchestration service<br />Create pipelines across clouds and on-premises data centers |

### ...

...




Section 28

252





Section 29

257



Section 30

270





Section 31

278





Section 32

281





# Section 33

### Architect: Understand Business Requirements

+ Business Requirements: Define what your business needs
+ Examples of Business Requirements:
  + Reduce costs (Understand Capital Expenditure vs Operational Expenditure)
    + Managed Services
    + Autoscaling
    + Preemptible VMs
  + Increase pace of innovation
    + Evaluate and adopt emerging processes like DevOps (CI/CD) and SRE
    + Evaluate and adopt emerging architectures like Microservices
  + Reduce mean time to recovery
  + Improve compliance with regulations
  + More visibility into applications and infrastructure (metrics)
  + More intelligence from the available data

### Total Cost of Ownership

+ Total Cost of Ownership (TCO) includes
  + Licensing Costs (Software + Hardware)
  + Computing costs
  + Storage costs
  + Networking costs (connection cost + Data ingress + Data egress)
+ 

### Defining Technical Requirements

+ Technical aspects that your system must adhere to
  + Functional
    + Must use containers
    + Must use hardened Linux OS
    + Needs container orchestration
    + Must be auto scale
  + 

### Planning for High Availability

| service/feature                 | features/best practices                                      |
| ------------------------------- | ------------------------------------------------------------ |
| Geographical Distribution       | Avaiability: Global > Multi-Regional > Regional > Zonal      |
| Compute Engine                  | Live Migration<br />Managed Instance Groups with AutoScaling and Health Checks (auto healing)<br />Distribute instances across regions/zones and distribute using Global Load Balancing |
| GKE                             | Multi master, Regional clusters with pod and cluster autoscaling |
| Managed Services                | Use Managed Services like App Engine, Cloud Functions, Cloud Storage, Cloud Filestore, Cloud Datastore, BigQuery |
| Persistent Disks                | Live resizing improves availability<br />Use Regional Persistent Disks |
| Cloud Bigtable                  | Place clusters in different zones or regions<br />(Each cluster belongs to a zone and you can create multiple clusters for high availability in the same instance) |
| Cloud Datastore                 | Use multi-region locations                                   |
| Cloud SQL                       | Use **HA configuration** (regional) - Cluster with primary instance and a standby instance is created<br />Read replicas will NOT be promoted (so read replicas do NOT increase availability for Cloud SQL) |
| **Network Tier**                | **Prefer Premium Network Tier** to Standard Network Tier     |
| **Hybrid Network Connectivity** | Speed and Availability: Dedicated interconnect > Partner interconnect > VPN<br />Have a backup connection<br />Example: VPN can be a backup for Dedicated interconnect or have multiple Dedicated interconnect connections |


### Planning for Scalability

+ **High Scalable Compute Engine Architecture** : VMs in MIG configured by instance template (and load balanced with Load Balancing)
  + (Remember) Unmanaged Instance Groups do not support Auto Scaling
+ Use **Pod and Cluster Autoscaling** for GKE

  > 1. Pod autoscaling: increase the number of instances of a specific microservice
  > 2. Cluster autoscaling: increase the number of nods in a cluster based on the number of pods you'd want to run
+ Be cautious with resources that cannot scale fast
  + Cloud SQL does NOT scale horizontally (only vertically)
+ Stateful applications are more difficult to scale than Stateless applications 
  + You can move state to a cache (like Memorystore) or a database

### Planning for Scalability - 2

+ **Persistent Disks**: Can be scaled horizontally and vertically
  + You can increase the size of PD while it is attached to a VM
  + You can attach new PDs (up to a max of 128) while a VM is running
+ **Compute**:
  + Cloud storage, app engine and cloud functions are serverless (Auto Scaling)
+ **Databases**:
  + kPub/Sub, BigQuery and Cloud Datastore are serverless (Auto Scaling)
  + BigTable, Cloud Spanner, Cloud SQL, Dataproc are **NOT serverless**
    + You need to choose the number of nodes in the cluster (and also the type of node for Cloud SQL and Dataproc)

### Planning for Security <p style="color: yellow;">CIA</p>

+ **Confidentiality** - Only right people have the right access
  + Follow **IAM best practices**
  + Encryption at rest and in transit (along with physical controls)
    + (DEFAULT) GCP encrypts all data at rest
+ **Integrity** - Protect data from unauthorized change
  + Follow **IAM Best Practices**
    + Role Based Access
    + Separation of duties
  + Hash verifications and digital signatures
+ **Availability** - Systems/Data is/are available when users need them
  + Solutions: Firewalls, Redundancy, Automatic Failover etc
  + Protect from Denial of Service (DoS) attacks

### Implementing DDoS Protection and Mitigation

+ **(DDoS) attack**: Attempting to bring your apps down with large scale attacks
+ **Shared responsibility** between GCP and Customer
  + GCP provides certain features to protect your app from DDoS attacks
    + **Anti-spoofing** protection for the private network
    + **Isolation** between virtual networks
    + App Engine (sits behind Google Front End) automatically protects you from Layer 4 and below attacks
      + Examples: SYN floods, IP fragment floods, prot exhaustion, etc.
  + What you can do to protect apps from DDoS attacks?
    + **Reduce the attack surface**: Make use of subnetworks and networks, firewall rules and IAM
    + **Isolate** your internal traffic
      + Use private IP address (unless you need public IP addresses)
      + Use private load balancing for internal traffic
    + **Use Proxy-based Load Balancing**: 
    + Integrate Load Balancing with **Cloud Armor**
      + IP-based and geo-based access control
      + Enforce Layer 7 security policies on hybrid and multi-cloud deployments
      + Pre-configured WAF rules (OWASP10)

### Digital Signatures - Cloud KMS

+ When do we use digital signatures?
  + Purpose 1: Verification of the integrity of the signed data
  + Purpose 2: Non-repudiation if the signer claims the signature is not authentic
    + Sender cannot deny sending the message later
+ Workflow:
  + Sender performs private key operation over the data to create a digital signature
  + Recipient uses the public key to verify the digital signature
    + If verification is unsuccessful, then the data has been altered
+ Cloud KMS can be used to create an asymmetric key pair (public and private key pair) that supports digital signing
  + Provides APIs/commands to create digital signatures
    + `gcloud kms asymmetric-sign`
+ Use cases: Validating code builds

### Cloud Armor

+ **Cloud Armor**: Protect your apps from denial of service and OWASP Top 10 attacks
  + Protects you from common web attacks (OWASP Top 10) like XSS (cross-site scripting) and SQL injection
  + Protect applications deployed in Google Cloud, in a hybrid deployment, or in a multi-cloud architecture
  + Provides **preconfigured security policies** (OWASP Top 10 risks etc)
    + Customize rules as per your needs
  + **Use cases**
    + Enable access for users at specific IP addresses with allow lists
    + Block access for users at specific IP address with deny lists
    + Protect applications against OWASP Top 10 risks

### Secret Manager

```java
SecretVersionName secretVersionName = SecretVersionName.of(projectId, secretId, versionId);
AccessSecretVersionResponse response = client.accessSecretVersion(secretVersionName);
String secretValue = response.getPayload().getData().toStringUtf8();
```

+ How do you manage your database passwords, your API keys securely?
+ **Secret Manager** - Store API keys, passwords etc
  + Multiple versions of secrets
  + Automate rotation
  + Auditing with Cloud Audit Logs
  + Encrypted by default
+ Best Practice: **Do NOT store credentials/passwords in code**
  + Use Secret Manager and access secrets in your application

### Architect Responsibilities: Stakeholder Management

+ **Stakeholder**: "a person with an interest or concern in something"
  + Different stakeholders have different priorities/interests/degrees of influence:
    + CEO/CTO
    + Business Process Owner
    + Product Owner
    + Architecture Team
    + Scrum Master
    + Development Team
    + Test Team
+ As an architect, you need to **understand the needs of different stakeholders and design solutions** that meet them
  + Key to stakeholder management:**Early clear communication**
    + Identify stakeholders, their interests, and build a plan to effectively communicate with them

### Architect Responsibilities: Change Management

+ Changes are inevitable
  + People, process, and technology (systems, hardware, and software)
  + But humans hate change
+ Change Management: How do you deal with changes and reduce impact?
  + Understand the change: 
    + Reason
    + Risks
    + Resouces
    + Responsibility: Who can make it happen?
  + Follow the cycle: Plan - Do - Check - Action

### Architect Responsibilities: Business Continuity Planning

+ BCP: How to keep business running in face of disasters?
+ **Disaster Recovery**: Parts of BCP focused IT operations
  + Example: use cloud/on-prem as DR environment
  + Example: have a backup network connection between cloud and on-prem
  + Disaster Recovery Plan: How should an enterprise respond to different types of disasters?
    + Includes people, process and technology (systems, hardware, and software)
    + Identifies critical services, personnel and plans
    + Define recovery time objective (RTO) and Recovery Point Objective (PRO)
      + Design and Architect your solutions to meet your RTO and RPO objectives
    + DR plans should be **continously tested** (Game days)
      + Are you able to restore a database from archive?
  + **BCP = Disaster Recovery Plan** (Restore critical IT applications and systems) + **Business recovery** (Restore critical business processes)

### Architect Responsibilities: Incident Management

+ **Incident**: "unplanned event that causes a service disruption"
+ **Goal**: "How to avoid incidents?" and "How to react to incidents quickly?"
+ **How to avoid incidents?**
  + Monitoring and Alerting
    + Examples: Alerts: free disk space in Storage/Databases, CPU utilization, Size of message queue
  + Programming best practices
    + Retry with exponential backoff
+ **How to react to incidents quickly?**
  + Monitoring (logging, tracing debugging, metrics, dashboards)
  + Being prepared (game days)
+ **Post-mortem**
  + Goal: how to prevent a repeat of the incident?
    + Goal is NOT "Whom to blame"

### Architect Responsibilities: Data Management

+ Managing data and its flow:
  + How does data come in?(stream or batch or transactional application)
    + If you have plans to move from batch to stream, prefer Cloud Dataflow
  + What rate will we receive data?
    + Plan for scaling storage (prefer auto scaling storage like Cloud Storage)
  + What kind of data? (archive or ...)
  + How much data? (GB, TB, PB)
  + For how long?
    + Cloud Storage Lifecycle Policies and Retention Policies
    + Table Expiration
  + Who will have access?
  + How will you use the data?
    + Ideally keep data and data processing system in the same zone (or atleast the same region)
    + If you are not planning to use the data for a long time, use Cloud Storage Archive Storage class

# Section 34

### Cloud Scheduler

+ Fully managed, enterprise-grade scheduler
  + Schedule all kinds of jobs
  + Uses Unix cron format
+ Integrates with App Engine, Cloud Pub/Sub, Cloud Logging and any HTTP endpoint
+ Manage all your automation tasks from one place
+ Provides automated retries
+ Use cases: Schedule tasks across a fleet of Compute Engine instances
  + Use Cloud Scheduler for scheduling a message on Pub/Sub
  + Compute Engine instances can process messages from Pub/Sub
+ (Remember) Needs an App Engine App in the Project
  + Earlier Alternative: App Engine Cron Service

### Cloud Emulators

+ How do you develop GCP applications in your local machine without connecting to GCP?
  + Setup local development environment with Cloud Emulators
+ Supports **emulation of**:
  + 
+ You can develop your applications locally using emulators!

### Cloud DNS

+ What would be the steps in setting up a website with a domain name (for example, wysiwyz.com)?
  1. Buy the domain name in wysiwyz.com (Domain Registrar)
  2. Setup your website content (Website Hosting)
  3. Route requests to wysiwyz.com to my website host server (DNS)
+ Cloud DNS = Global Domain Name System (step#3)
  + Setup your DNS routing for your website (wysiwyz.com)
    + Route api.wysiwyz.com to the IP address of api server
    + Route static.wysiwyz.com to the IP address of http server
    + Route email ( oregano@wysiwyz.com ) to the mail server (mail.wysiwyz.com)
  + Public and private managed DNS zones (container for records)

### Cloud DNS - CLI

+ `gcloud dns managed-zones create ZONE_NAME`
  + `--description`
  + `--dns-name` 
  + `--visibility` (private / public)
  + `--networks` 
+ Three steps to add records to a managred zone:
  + Start Transaction for Zone
  + Make Changes
    + `gcloud dns record-sets transaction add --name`
  + End Transaction for Zone

### Pricing Calculator

+ Estimating the cost of a Google Cloud solution is NOT easy
+ You would need to take a number of factors into account
+ How do you estimate the cost of your GCP solution?

### Anthos

+ Run K8S cluster on cloud and on-premises
  + Multi-cluster management: Consistent managed K8S
  + Consistent development and operations experience
+ Centralized config management (Git repo)
  + Logically group and normalize clusters as environs
    + Define policies - Kubernetes API, Access control
  + Deploy to clusters on new commits
    + Use namespaces, labels and annotations to decide which clusters to apply changes on
+ Provides **Service Mesh** (based on **Istio**)
  + **Sidecar** to implement common microservice features
    + Authentication and authorization (service accounts)
    + Distributed Tracing, Automatic metrics, logs and dashboards
    + A/B testing, canary rollouts (even trak SLIs and error budgets)
    + Cloud Logging and Cloud monitoring

### Machine Learning in Google Cloud

| Service                        | Discussion                                                   |
| ------------------------------ | ------------------------------------------------------------ |
| Prebuilt APIs                  | Needs no in-house ML expertise. Easy to Use.<br />Examples: Vision API, Video API, Natural Language API, Speech-to-Text API, Text-to-Speech API, and Translation API |
| Cloud AutoML                   | Build custom ML models with developers having limited ML expertise |
| AI Platform                    | Help data scientists build custom models (based on Tensorflow Enterprise)<br />Serverless, scalable training and serving capabilities for custom ML models<br />Take ML projects from concept to production quickly<br />Explainable AI - interpret models with confidence |
| Data management                | Cloud Storage and BigQuery<br />BigQuery ML - ...            |
| Automation and instrumentation |                                                              |

### REST API Challenges

+ Most applications today are built around REST API:
  + Resources (/todos, /todos/{id}, etc.)
  + Actions - HTTP methods - GET, PUT, POST, DELETE etc.
+ Management of REST API is not easy:
  + You've to take care of authentication and authorization
  + You've to be able to set limits (rate limiting, quotas) for your API consumers
  + You've to take care of implementing multiple versions of your API
  + You would want to implement monitoring, caching and a lot of other features...

### Apigee API Management

+ Design, secure, publish, analyze, monitor, monetize and scale APIs anywhere
  + On-premises, Google Cloud, or Hybrid
+ Manage **Complete API life cycle**
+ Provides AI-powered API monitoring (Get actionable insights)
+ Enable Caching with Cloud CDN
+ Create **Developer Portals**:
  + Allow developers to easily explore the APIs, get API keys etc. 
    + e.g. Apigee integrated portal
+ Use Case: Abstraction layer on top of legacy services
+ Use Case: Expose your assets (ML Models) as APIs

### Getting Started with Identity Platform

+ Identity Platform: Customer identity and access management
+ What's the difference: Cloud IAM vs Identity Platform
  + Cloud IAM: Employees and Partners Authorization
    + Control access to Google Cloud Resouces
    + Member, Roles, Policy, Service Accounts
  + Identity Platform: Customer identity and access management (CIAM)
    + Authentication and Authorization for your applications and services
+ Identity Platform: Key Features
  + Authentication and authorization for web and mobile apps (iOS, Android, ...)
  + Multiple authentication methods
    + 
  + Features: User sign-up and sign-in, MFA etc
  + An upgrade from Firebase Authentication Legacy
  + Integrates well with Identity-Aware Proxy

### Cloud IAM vs Identity Platform - Scenarios

| Scenario                                                     | Solution                    |
| ------------------------------------------------------------ | --------------------------- |
| An Application on a GCE VM needs access to cloud storage     | Cloud IAM - Service Account |
| An enterprise user need access to upload objects to a Cloud Storage bucket | Cloud IAM                   |
| I want to manage end users for my application                | Identity Platform           |
| I want to enable "Login using Facebook/twitter" for my application | Identity Platform           |
| I want to create user sign-up and sign-in workflows for my application | Identity Platform           |

### Getting Started with Event Driven Architecture

+ Microservices calling each other => Tight Coupling
+ Event driven architectures: Microservices reacting to changes in state (events)
  + Example:
    + 1: Order Service publishes an OrderReceived event
    + 2: Billing Service receives it and publishes an OrderBilled event
    + 3: Warehouse Service receives it and publishes an OrderReadyToShip event
    + 4: Shipping Service receives it and publishes and OrderShipped event
    + 5: Email Service receives it and sends an email to the user
  + Advantages:
    + Loose Coupling: Microservices do not know about each other
    + Flexible Orchestration: Same event can be processed by multiple services
    + Resiliency: Events can be easily retried in case of failures
    + Asynchronous: A microservice does not need to wait for the consumer to process the event

### Getting Started with CloudEvents

+ CloudEvents: Standard specification to describe events
  + Reference:
  + Challenge: different publishers use different formats for events
  + Goal: Describe event data in a standard way
    + A project under CNCF - Cloud Native Computing Foundation
  + Advantages:
    + 
    +  

#### Getting Started with Eventarc

+ Eventarc: Simplifies event driven architectures in Google Cloud
  + Adheres to the CloudEvents (cloudevents.io) specification
  + Event provider: Who can trigger events?
    + Direct: ...
    + In-Direct: From Cloud Audit Logs Entries
      + Huge variety of Google Cloud services. Example: GCE, GAE, Artifact Registry ...
  + Event destination: Who can process events?
    + Cloud Functions (2nd gen), Cloud Run and GKE services
  + (BACKGROUND) Uses Pub/Sub topics

### Getting Started with Observability and OpenTelemetry

+ **Observability**: "measure the internal state of a system by examining its outputs"
  + **Goal**: Proactively identify problems and fix them
  + THREE PILLARS of observability: logs, metrics and traces
    + Earlier we ahd different standards for logs, metrics and traces
    + We also have very different approaches across languages
  + How about **ONE STANDARD ACROSS PLATFORMS**?
    + OpenTelemetry:  ...
      + Open standard
      + A CNCF - Cloud Native Computing Foundation - Project (K8S is another CNCF project)
      + Almost every cloud platform supports OpenTelemetry
      + Steps to use OpenTelemetry

### Using Service Directory

+ Service Discovery - Help microservices find one another
+ Service Directory - A single place to publish, discover and connect services
+ Your worklaods can be running in:
  + Google Cloud
  + d
  + d



### Using Service Directory - Features

+ Managed Service (highly available and scalable)
+ Register/resolve services using DNS, HTTP, and gRPC
+ Service Directory client libraries are available for multiple languages (along with REST/RPC APIs)
  + You can use these libraries to register/resolve service location
+ Audit logging ("Who did what, where and when?" - Cloud Logging)
+ Request/response logs(Cloud Logging)

# Section 35

### Google Cloud Architecture Framework

+ Best praces and implementation recommendations to help you design your Google Cloud deployment
+ First Step:
+ 4

### Principle 1: Operational Excellence

+ "Efficiently running, managing and monitoring systems that deliver business value"
+ Strategies:
  + 

### Operational Excellence - Best practices

+ Increase software development and release velocity
  + Release Engineering
    + Frequent small releases
  + Automation
    + 
  + Services:
    + 

### Operational Excellence - Best practices - 2

+ Monitor system health and business health
  + Understand four golden signals
    + Latency
    + Traffic
    + Errors
    + Saturation
  + Logging: Use Cloud Logging and export to Cloud Storage, BigQuery and Pub/Sub depending on your needs
  + Metrics: Define and capture metrics SLIs, SLOs etc
  + Monitoring: Cloud Monitoring can aggregate metrics, logs, and events
    + Define alerts and custom metrics to identify problems and resolve them quickly
    + You can create visual dashboards as well
    + Generate actionable alerts
  + Key Services: Cloud Monitoring, Cloud Logging, Cloud Debugger, Error Reporting, Cloud Trace and Cloud Profiler



### Operational Excellence - Best practices - 3

+ Design for disaster recovery
  + Create well-defined disaster recovery (DR) plan
    + Define Recovery time objective (RTO) and recovery point objective (RPO)
    + Consider everything in your DR plan: Applications (comput etc), Data (databases etc), Network infrastructure, Bandwidth, Facilities
    + Use features provided by Google Cloud:
      + Use Global network to build Redundancy
      + Managed services make Scalability easy
  + Regularly test your DR plan
  + Example features you can make use of:
    + Schedule Persistent Disk snapshots for your VMs and copy them across regions
    + Enable Live Migration to keep your VMs running even when there is software or hardware maintenance
    + Use Cloud DNS to switch from primary to backup

### Principle 2: Security, Privacy and Compliance

+ Plan your security controls, privacy, and meet your compliance needs
+ Strategies
  + Implement least privilege with identity 

### Security, Privacy and Compliance - Best practices

+ Manage Authentication and Authorization
  + Follow Identity and Access Management (IAM) Best Practices
    + Grant appropriate roles
    + Understand when to use service accounts
      + Treat each app component as a separate trust boundary
        + Create separate service account for each component/service with the minimum permissions needed
    + Use Organization Policy Service (what is allowed in your organization?)
    + Use Cloud Asset Inventory (Track your inventory)
    + Use Cloud Audit Logs to audit IAM policy changes and service accounts
+ Implement Compute Security Controls
  + Use Private IPs as much as possible
  + Create hardened VM images (OS + minimum software)
  + Use Shielded VMs to prevent remote attacks, privilege escalation, and malicious insiders
  + Enable node auto-upgrades for GKE clusters
  + Use GKE Sandbox 

### Security, Privacy and Compliance - Best practices - 2

+ Secure the network
  + Use a carefully designed custom VPC (DO NOT use the default VPC)
  + Isolates workloads into individual projects by creating one VPC per project
  + Control ingress and egress network traffic using firewall rules
  + Run advance security and traffic inspection tools
  + Use Security Command Center to analyze the security of your infrastrature
  +  

### Security, Privacy and Compliance - Best practices - 3

+ Implement data security controls
  + (Default) 
  + Use Object Versioning in Cloud Storage for sensitive data
  + Implement data security
+ Audit your infrastructure with audit logs
  + Use Cloud Audit Logs
  + Enable Access 

### Principle 3: Reliability

+ Most important feature of any application/service:
  + Keys:
    + Measurable reliability goals
    + Architect for scalability, high availability, and automated change management
    + Self-healing and observability are important
    + Quick recovery from failures using automations as much as possible
  + Strategies:
    + Define KPIs, SLOs and SLIs
    + Create redundancy (Prefer horizontal scalability)
    + Make incremental changes
      + include rollback capability
    + Instrument systems for observability
    + Automate detection of failure
    + Document and automate emergency responses
    + Test failure recovery
    + Reduce toil (Eliminate manual work)

### Reliability - Best practices

+ **Define reliability goals**: Service Level Objectives and Error Budgets
  + Clearly define SLls, SLOs, SLAs and Error budgets
+ **Build observability** into your infrastructure and applications
  + Monitoring, logging, tracing, profiling, debugging etc.
+ **Design for scale and high availability**
  + Design a multi-region architecture with failover
  + Eliminate scalability bottlenecks
    + Prefer horizontal scalability
  + Degrade service levels gracefully
    + Static web pages when dynamic web site is down
    + Temporarily disabling data updates
  + Implement exponential backoff with jitter
    + Exponentially increasing wait time before retry
  + Predict peak traffic events and plan for them

### Reliability - Best practice - 2

+ Build flexible and automated deployment capabilities
  + Ensure that every change can be rolled back
  + Spread out traffic for time promotions and launches
  + Implement progressive rollouts with canary testing
  + Automate build, test and deploy
+ Build efficient alerting
+ Build a collaborative process for incident management
  + Reduce Mean Time to Detect (MTTD): Alert teams at the right time
  + Reduce Mean Time to Mitigate (MTTM) and Mean Time to Recovery (MTTR): Properly documented and well-exercised incident management plan
  + Increase Mean Time Between Failures (MTBF): Build reliable systems
  + Blameless postmortem culture

#### Principle 4: Performance and Cost Optimization

+ Strategies
  + Evaluate performance requirements
  + Use scalable design patterns
  + f
+ Use autoscaling and data processing
  + Autoscale Compute Engine VMs with Managed instance groups (MIGs) - Uses an instance template
  + Enable Cluster autoscaler and Pod
  +  

### Performance and Cost Optimization - Best practices - 2

+ Identify apps to tune
  + Use Cloud Trace, Cloud Debugger and Cloud Profiler to gain insights into your apps
  + Instrument apps to identify inter-service
+ Analyze your costs and optimize
  + 

# Section 36

### Case Studies - *Important*

### 1. EHR Healthcare

+ Provides SaaS based electronic health record software for multi-national medical offices, hospitals and insurance providers (Exponential growth):
+ Current Architecture
  + Multiple colocation facilities with similar but separate environments (lease on one data center is about to expire)
    + Web-based customer-facing applications running on Kubernetes clusters
      + Requirement: Consistent way to manage apps that are container-based
      + Requirement: Maintain and manage multiple container-based environments
    + Databases: MySQL, MS SQL Server, Redis and MongoDB
    + Legacy file- and API-based integrations with insurance providers
      + Scheduled for replacement over next few years - Will NOT be moved to Google Cloud
      + Requirement: Main legacy interfaces to insurance provides (connectivity to on-premises and cloud)
    + Users are managed via Microsoft Active Directory
    + Monitoring with various open source tools
    + Alert by email (generally ignored)

### Requirements

+ Google Cloud will replace their current colocation facilities
+ Scalable, resilient platform spanning multiple environments seamlessly
+ Facing outages with mis-configured systems, inadequate capacity to manage spikes and inconsistent monitoring practices
+ Other Requirements:
  + Dynamically scale and provision environments, adapt DRP, continuous deployment
  + Centralized visibility and proactive action on system performance and usage
  + On-board new insurance providers quickly
  + Insights and predictions
  + Secure and high-performance connection between on-premises system

| service                                      | discussion                                                   |
| -------------------------------------------- | ------------------------------------------------------------ |
| Anthos                                       |                                                              |
| Cloud Logging, Cloud Monitoring              | Alerting Policies for Notifications                          |
| Cloud Logging > (Cloud Storage, BigQuery)    | Log Retention                                                |
| BigQuery                                     | Make predictions and generate                                |
| Cloud Dedicated Interconnect                 | Secure and high-performance connection                       |
| Cloud CDN                                    | Reduced latency                                              |
| Active Directory Federation Services (AD FS) | For single sign-on<br />Google Cloud Directory Sync (synchronize user and groups) ... |
| Databases                                    |                                                              |



### 2. Helicopter Racing

+ Global sports league for competitive helicopter racing
+ Current Architecture:
  + Existing public cloud provider (public cloud-first company) hosts:
    + Core of mission-critical apps
    + Existing content is stored in an object storage service
    + Race predictions are performed using TensorFlow running on VMs
      + Allows predicting race outcomes but lacks the facility to:
        + Support real-time predictions during races (REQUIREMENT)
        + Capacity to process season-long results (REQUIREMENT)
  + Video recording and editing is performed at the race tracks:
    + Content is encoded and transcoded in the cloud (using VMs created for each job)
  + Enterprise-grade connectivity and local compute provided by truck-mounted mobile data centers

### Requirements

+ Migrate existing service to a new platform
  + Expand use of managed AI and ML services to facilitate race predictions:
    + Increase predictive capabilities during and before races:
      + Race results, Mechanical failures, Crowd sentiment
      + Enhanced video streams that include predictions of events within the race (e.g. overtaking)
      + Maintain or increase prediction throughout and accuracy
    + Expose the predictive models to partners
    + Create real-time analytics of viewer consumption patterns and engagement
      + Increase telmetry and create additional insights
      + Measure fan engagement with new predictions
  + Move the serving of real-time and recorded content closer to their users:
    + Reduce latency, enhance global availability and quality of the broadcasts
    + Increase the number of concurrent viewers
+ **Create a data mart** to enable processing of large volumes of race data
+ Increase transcoding performance
+ Create a merchandising revenue stream

### Discussion

| service                                   | discussion                                                   |
| ----------------------------------------- | ------------------------------------------------------------ |
| BigQuery + AI Platform                    | Migrate existing service to a new platform (Managed AI and ML service using TensorFlow) |
| Cloud Storage, Cloud CDN                  | Move the serving of real-time and recorded content closer to their users |
| Apigee                                    | Expose the predictive models to partners<br />               |
| Cloud Pub/Sub                             |                                                              |
|                                           |                                                              |
| Video Intelligence API<br />Streaming API | Real-time streaming analysis for live media<br />AIStreamer ingestion library allows you to connect to API<br />Supports File Streaming, HTTP Live Streaming (HLS), Real Time Streaming Protocol (RTSP), Real Time Messaging Protocol (RTMP) |
| References                                |                                                              |



### 3. Mountkirk Games

+ Online, session-based, multi-player games for mobile platforms
  + Longterm Requirement: Support multiple games platforms (expand beyond mobile)
+ Current Architecture:

### Requirements

+ Working on **retro-style first-person shooter (FPS)** game
  + Hundreds of simultaneous players joining a geo-specific digital arena from multiple platforms and locations
  + **Proposed**: Run game backend on Google Kubernetes Engine (Rapid Scaling)
    + Google's global load balancer to route to closest regional game arenas
    + Support eventual migration of legacy games to this new platform
    + Requirement: Real-time global leaderboard of top players across every active arena
      + Proposed: Use multi-region Spanner cluster for near real-time global leader board
+ **Other Requirements**:
  + Store game activity logs in structured files for future analysis
  + Use GPU processing to render graphics server-side for multi-platform support
  + Enable advanced analytics capabilities
  + Use cloud-native design principles
    + Multiple regions, Rapid interation, Minimize latency and costs, Auto scaling
    + Managed services and Pooled resources

### Discussion

| service                                                      | discussion                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Game Servers (Agones + Kubernetes)                           | Deliver seamless multiplayer gaming experiences              |
| MIGs + VMs + Global Load Balancing + GPUs                    | If you need a lot of customization                           |
| Cloud Storage                                                | Store game activity logs in structured files for future analysis |
| Cloud Spanner                                                | Proposed real-time global leader board<br />*prefer Memorystore - Redis |
| Batch - Cloud Storage > Dataflow > BigQuery Streaming - Pub/Sub > Dataflow > BigQuery | Enable advanced analytics capabilities<br />Explore Looker for advanced insights |
| Cloud Build, Spinnaker and/or Jenkins                        |                                                              |
| Cloud Spanner + Memorystore                                  |                                                              |
| Cloud Bigtable                                               |                                                              |
| Cloud Firestore                                              | Player database                                              |
| Cloud BigQuery, AI Platform                                  |                                                              |
| Refereences                                                  | https://www.youtube.com/watch?v=dAKAWprvbso<br />https://www.youtube.com/watch?v=OF8EtzQ4e6c<br /> |



#### 4. TerramEarth

+ **Manufactures heavy equipment** for the mining and agricultural industries
  + Mission: Build products that make their customers more productive
  + 500 dealers and service centers in 100 countries (Global audience)
  + 2 million TerramEarth vehicles with 20% growth year on year
    + Sensors capture telemetry data
    + Subset of critical data transmitted in real time
      + Rest of data collected, compressed, and uploaded daily when the vehicles return to home base
      + 200 to 500 MB of data per vehicle per day
+ **Current Architecture**
  + Google Cloud
    + Data aggregation and analysis infrastructure
    + Web frontend for dealers and customers (stock management and analytics)
  + Private data centers
    + Legacy inventory and logistics management systems
    + Two main manufacturing plants send sensor data to private data centers

### Requirements

+ Requirements:
  + Current:
    + Provide best-in-class online fleet management services to customers
      + Improve operations of dealerships
      + Minimize vehicle downtimes (Detect failures and rapidly ship parts to dealerships for just-in-time repair)
    + Autoscaling, DevOps and SRE Elements
      + Improve and standardize tools for app and network monitoring, troubleshooting
      + Modernize CI/CD pipelines for container-based workloads
      + Allow remote developers to be productive
    + Flexible and scalable platform for develoopers to create custom APIs for dealers and partners
      + Create a new abstraction layer for HTTP API access to legacy systems (Enable gradual move to cloud)
      + Self-service portal for internal and partner developers to create new projects, request resources for data analytic jobs and centrally manage access to the API endpoints
    + Use cloud-native solutions for keys and secrets management
  + 5-year strategic plan
    + Create a partner ecosystem of new products by enabling access to our data
    + Increase autonomous operation capabilities of vehicles
    + Path to move legacy systems to the cloud

### Discussion

| Service                                                      | Discussion                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| IOT Core > Pub Sub > Dataflow > BigQuery > (AI platform, AutoML, BigQuery ML) |                                                              |
| Cloud Storage > Dataflow > BigQuery                          | Batch Process                                                |
| Apigee                                                       | Flexible and scalable platform for developers to creat custom APIs <br />Create a new abstraction layer for HTTP API access to legacy systems (Enable gradual move to cloud)<br />Self-service protal for internal |
| Cloud Build, Spinnaker, Jenkins                              |                                                              |
| Google Workspace, Virtual destops through partners<br />     |                                                              |
| Secret Manager                                               |                                                              |
| VPC Flowlogs, Cloud Monitoring, Cloud Logging etc...         |                                                              |
| Kubernetes - GKE                                             | Container based workloads - scalable                         |
| Dedicated Interconnect                                       | Networking                                                   |

# Section 37

### GCA Resources

| Title                                                      | Link                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| Cloud Architecture Center                                  | https://cloud.google.com/architecture/                       |
| Google Cloud Solutions                                     | https://cloud.google.com/solutions/                          |
| Best Practices for Enterprise Applications                 | https://cloud.google.com/architecture/framework              |
| Building Scalable and Resilient Web Applications on Google | https://cloud.google.com/architecture/scalable-and-resilient-apps |
| Disaster recovery planning guide                           | https://cloud.google.com/architecture/dr-scenarios-planning-guide |
| Home                                                       | https://cloud.google.com/learn/certification/cloud-architect |
| Exam Guide<br />Case Studies                               | https://cloud.google.com/learn/certification/guides/professional-cloud-architect |
| Sample Questions                                           | https://docs.google.com/forms/d/e/1FAIpQLSf54f7FbtSJcXUY6-DUHfBG31jZ3pujgb8-a5io_9biJsNpqg/viewform |

### Certification Exam

+ 50 questions, two hours
+ Questions:
  1. multiple choice: 4 options, 1 right answer
  2. multiple select: 5 options, 2 right ansers
+ Result immediately shown after exam completion
  + email (2~5 work days)
+ Tips & suggestions:
  1. Read the entire question
     + identify and write down the key parts of the question
     + Focus on the constraint in the question
       + e.g. Most cost-effective way - Prefer region to multi-regional if it satisfies all requirements
  2. **Answer by Elimination**!
  3. Flag question for future consideration (Review before final submission) 

---

### Google Cloud API

#### API management tools

1. Cloud Endpoints
   + Distributed API management system
   + Provides an API console, hosting, logging, monitoring, and other features
   + Use with any APIs that support the OpenAPI Specification
   + Supports applications running in App Engine, Google Kubernetes Engine, and Compute Engine
   + Clients include Android, iOS, and Javascript
2. API Gateway
   + Backend implementations can vary for a single service provider
   + Provide secure access to your backend servics through a well-defined REST API
   + Clients consume your REST APIS to implement standalone apps
3. Apigee API Management
   + Specific focus on business problems, like rate limiting, quotas, and analytics
   + Many Apigee API Management users provide a software service to other companies
   + Backend services for Apigee API Management don't need to be in Google Cloud

#### Cloud Run

4. Cloud Run
   + A managed compute platform that can run stateless containers
   + Serverless, removing the need for infrastructure management
   + Built on Knative, an open API and runtime environment built on Kubernetes
   + Can automatically scale up and down from zero almost instantaneously, charging only for the resources used



​			
