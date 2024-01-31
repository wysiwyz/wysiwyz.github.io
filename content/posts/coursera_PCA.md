---
title: "GCP - preparing for your cloud architect journey"
date: 2023-05-21T09:10:20+08:00
author: "celine"
tags: ["coursera", "security"]
categories: ["StudyNote"]
---

> 2023-08-05
>
> Yeah, 考過了！
>
> 接下來要準備 OCPJP SE 11

## VPC

#### Projects, networks, subnetworks

+ A project
  + Associates objects and servicing with billing
  + Contains networks 
+ A network

#### 3 VPC network types

+ Default
  + Every project
  + One  subnet per region
  + Default firewall rules
+ Auto Mode
  + Default network
  + One subnet per region
  + Regional IP allocation
  + Fixed /20 subnetwork per region
  + Expandable up to /16
+ Custom Mode
  + No default subnets created
  + Full control of IP ranges
  + Regional IP allocation
  + Expandable to IP ranges you specify

#### Expand subnets without re-creating instances

+ Cannot overlap with other subnets

+ IP range must be a unique valid CIDR block

+ New subnet IP ranges have to fall within valid IP ranges

+ Can expand but not shrink

+ Auto mode can be expanded from /20 to /16 ip range

+ Avoid creating large subnets

  > Do not scale your subnet beyond what you actually need.

#### IP Addresses

+ Internal IP

  + Allocated from subnet range to VMs by DHCP
  + DHCP lease is renewed every 24 hours
  + VM name + IP is registered with network-scoped DNS

+ External IP

  + Assigned from pool (ephemeral)
  + Reserved (static)
  + Bring Your Own IP address (BYOIP)
  + VM doesn't know external IP; it is mapped to the internal IP

  

  ---

  #### External IPs are mapped to internal IPs

  #### DNS resolution for internal addresses

  + Each instances has a hostname that can be resolved to an internal IP address:

    + The hostname is the same as the instance name

    + FQDN is [hostname].[zone].c.[project-id].internal

      Example: *my-server.us-central1-a.c.guestbook-151617.internal*

  + Name resolution is handled by internal DNS resolver:

    + Provided as part of Compute Engine (169.254.168.254).
    + Configured for use on instance via DHCP
    + Provides answer for internal and external addresses

  #### DNS resolution for external addresses

  + Instances with external IP address can allow connections from hosts outside the project.
    + Users connect directly using external IP address
    + Admins can also publish public DNS records pointing to the instance
      + Public DNS records are not published automatically
  + DNS records for external addresses can be published using existing DNS servers (outside of Google Cloud)
  + DNS zones can be hosted using Cloud DNS

  #### Host DNS zones using Cloud DNS

  + Google's DNS service
  + Translate domain names into IP address
  + Low latency
  + High available (100% uptime SLA)
  + Create and update millions of DNS records
  + UI, command line or API

  

  #### Assign a range of IP addresses as aliases to a VM's network interface using alias IP ranges

  

  ---

  #### Routes and firewall rules

  ##### A route is a mapping of an IP range to a destination

  Every network has:

  + Routes that let instances in a network send traffic directly to each other
  + A default route that directs packets to destinations that are outside the network

  Firewall rules must also allow the packet.

  ---

  ##### Routes map traffic to destination networks

  + Apply to traffic egressing a VM.
  + Forword traffic to most specific route.
  + Are created when a subnet is created.
  + Enable VMs on same network to communicate.
  + Destination is in CIDR notation.
  + Traffic is delivered only if it also matches a firewall rule.
  
  ##### Firewall rules protect your VM instances from unapproved connections
  
  + VPC network functions as a distributed firewall
  + Firewall rules are applied to the network as a whole.
  + Connections are allowed or denied at the instance level.
  + Firewall rules are stateful.
  + Implied deny all ingress and allow all egress rule
  
  #### Google Cloud firewall use case: Egress
  
  + Conditions:
    + Destination CIDR ranges
    + Protocols
    + Ports
  + Action:
    + Allow: permit the matching egress connection
    + Deny: block the matching egress connection
  
  ---
  
  #### Virtual machines
  
  |x| Compute Engine   | GKE                | App Engine std env  | App Engine flexible env | Cloud Functions | Cloud Run |
  | ---------------- | ------------------ | ------------------- | ----------------------- | --------------- | --------- | ---|
  | Language support | Any                | Any                 | Py, Node.js, Go, Java, Ruby, PHP | Py, Node.js, Go, Java, PHP, Ruby, .NET, Custom ... | Py, Node.js, Go, Java |Any|
  | Usage model      | IaaS               | IaaS,<br />PaaS     | PaaS | PaaS | Microservices architecture |PaaS|
  | Scaling          | Server autoscaling | Cluster             | Autoscaling managed servers | Autoscaling managed servers | Serverless |Serverless|
  | Primary use case | General workloads  | Container workloads | Scalable web applications,<br />Mobile backend applications | Scalable web applications,<br />Mobile backend applications | Lightweight event actions |Deploy & scale containerized apps|
  
  #### Infrastructure as a Service
  
  + Predefined or custom machine types
    + vCPUs (cores) and Memory (RAM)
    + Storage
      + Zonal or regional persistent disk (HDD or SSD)
      + Local SSD
      + Cloud Storage
    + Networking
    + Linux or Windows
  
  #### Compute Engine features
  
  1. Machine rightsizing
     + Recommendation engine for optimum machine size
     + Cloud Monitoring statistics
     + New recommendation 24 hrs after VM create or resize
  2. (-)
     + Instance metadata
     + Startup scripts
  3. Availability policies
     + Live migrate
     + Auto restart
  4. Global load balancing
     + Multiple regions for availability
  5. OS patch management
     + Create patch approvals
     + Set up flexible scheduling
     + Apply advanced patch configuration settings
  6. (-)
     + Per-second billing
     + Sustained use discounts
     + Committed use discounts
  7. Preemptible and Spot VMs:
     + Up to 91% discount
     + No SLA

#### VM access

##### Linux: SSH

> The creator has SSH capability and can use the Cloud Console to generate a unsername and password.

+ SSH from the Google Cloud console or Cloud Shell via the Google Cloud SDK
+ SSH from computer or third-party client and generate key pair
+ Requires firewall rule to allow `tcp:22`

##### Windows: RDP

+ RDP clients
+ Powershell terminal
+ Requires setting the Windows password
+ Requires firewall to allow `tcp:3389`

```
[Machine family] 
    \            General-Purpose
      \          Compute-Optimized
        \        Memory-Optimized
          \      Accelerator-Optimized
       [Machine series]
             \
          [Machine type]
```

##### Availability policy : Automatic changes

Called "scheduling options" in SDK/API

Automatic restart

+ Automatic VM restart due to crash or maintenance event
  + Not preemption or a user-initiated terminate

On host maintenance

+ Determines whether host is live-migrated or terminated due to a maintenance event.
  Live migration is the default

Live migration

+ During maintenance event, VM is migrated to different hardware without interruption
+ Metadata indicates occurrence of live migration

##### Patching management is an essential part of managing an infrastructure

Manage OSes easily through Google Cloud

+ Keep infrastructures up-to-date
+ Redue the risk of security vulnerabilities

Two main components of <span style="color:skyblue">OS patch management service</span>:

1. Patch compliance reporting
   + reports insights on the patch status of your VM instances across Windows and Linux distributions
2. Patch deployment
   + automates the Operating System and software patch update process

##### There are several tasks that can be performed with patch management

You can ...
    Create patch approval
    Set up flexible scheduling
    Apply advance patch configuration (e.g. pre/post patch scripts)
    Manage these patch jobs or updates from a centralized location

##### Charges for stopped (terminated) VM

| No charges    | Charges ($$)          |
| ------------- | --------------------- |
| Memory        | Attached disks        |
| CPU resources | Reserved IP addresses |

> Virtual Machines
> Note: Notice that you cannot change the machine type, the CPU platform, or the zone.
>
> You can add network tags and allow specific network traffic from the internet through firewalls. Some properties of a VM are integral to the VM, are established when the VM is created, and cannot be changed. Other properties can be edited.
>
> You can add additional disks and you can also determine whether the boot disk is deleted when the instance is deleted.
>
> Normally the boot disk defaults to being deleted automatically when the instance is deleted. But sometimes you will want to override this behavior. This feature is very important because you cannot create an image from a boot disk when it is attached to a running instance.
>
> So you would need to disable *Delete boot disk when instance is deleted* to enable creating a system image from the boot disk.

##### Availability policies

> **Note:** You cannot convert a non-preemptible instance into a preemptible one. This choice must be made at VM creation. A preemptible instance can be interrupted at any time and is available at a lower cost.
>
> If a VM is stopped for any reason, (for example an outage or a hardware failure) the automatic restart feature will start it back up. Is this the behavior you want? Are your applications idempotent (written to handle a second startup properly)?
>
> During host maintenance, the VM is set for live migration. However, you can have the VM terminated instead of migrated.
>
> If you make changes, they can sometimes take several minutes to be implemented, especially if they involve networking changes like adding firewalls or changing the external IP.

#### Compute options

##### Four Compute Engine machine families

1. General purpose

   + the best price performance; the most flexible vCPU to memory ratios

     | series      | workload                                                     | applications                                                 |
     | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | E2          | (Cost-optimized) Day-to-day computing at a lower cost        | Web/App serving; Back office apps; Small-medium DBs; Microservices; Virtual desktops; Dev environments |
     | N2, N2D, N1 | (Balanced) Balanced price/performance across a wide range of VM shapes | Web/App serving; Back office apps; Medium-large DBs; Cache; Media/streaming |
     | Tau T2D     | (Scale-out optimized) Best performance/ cost for scale-out workloads | Scale-out workloads; Web serving; Containerized microservices; Media transcoding; Large-scale Java apps |


2. Compute-optimized

   | Series | Workload                                               | Applications                                                 |
   | ------ | ------------------------------------------------------ | ------------------------------------------------------------ |
   | C2     | Ultra high performance for compute-intensive workloads | Compute-bond workloads; High-performance web serving; Gaming (AAA game servers); Ad serving; High-performance computing (HPC); Media transcoding; AI/ML |
   | C2D    | Ultra high performance for compute-intensive workloads | Memory-bound workloads; Gaming (AAA game servers); High-performance computing (HPC); High-performance DBs; Electronic Design Automation (EDA); Media transcoding |

3. Memory-optimized machine family

   | Series | Workload                    | Applications                                                 |
   | ------ | --------------------------- | ------------------------------------------------------------ |
   | M1     | Ultra high-memory workloads | Medium in-memory DBs such as SAP HANA; Tasks that require intensive use of memory with hight memory-to-vCPU ratios than the general-purpose high-memory machine types; In-memory DBs and in-memory analytics, business warehousing (BW) workloads, genomics analysis, SQL analysis services; MS SQL Server and similar DBs |
   | M2     | Ultra high-memory workloads | Large in-memory DBs such as SAP HANA; In-memory DBs and in-memory analytics, business warehousing (BW) workloads, genomics analysis, SQL analysis services |

4. Accelerator-optimized

   | Series | Workload                                           | Applications                                                 |
   | ------ | -------------------------------------------------- | ------------------------------------------------------------ |
   | A2     | Optimized for high-performance computing workloads | CUDA-enabled ML training and interface; HPC; Massive parallelized computation |

##### Create custom machine types

When:

+ Requirements fit between the predefined types
+ Need more memory or more CPU

Customize the amount of memory and vCPU for your machine:

+ Either 1vCPU or even number of vCPU
+ 0.9 GB per vCPU, up to 6.5 GB per vCPU (default)
+ Total memory must be multiple of 256 MB

#### Disk options

##### Boot disk

+ VM comes with a single root persistent disk
+ Image is loaded onto root disk during first boot:
  + Bootable: you can attach to a VM and boot from it
  + Durable: can survive VM terminate
+ Some OS images are customized for Compute Engine
+ Can survive VM deletion if "Delete boot disk when instance is deleted" is disabled

##### [1] Persistent disks

Network storage appearing as a block device

+ Attached to a VM through the network interface
+ Durable storage: can survive VM terminate
+ Bootable: you can attach to a VM and boot from it
+ Snapshots: incremental backups
+ Performance: Scales with size
+ HDD (magnetic) or SSD (faster solid-state) options
+ Disk resizing: even running and attached!
+ Can be attached in read-only mode to multiple VMs
+ Zonal or Regional
  + Zonal - offer efficient, reliable block storage
  + Regional - provide active-active disk replication across two zones in the same region
  + `pd-standard`
  + `pd-ssd`
  + `pd-balanced`
  + `pd-extreme`(zonal only)
+ Encryption keys
  + Google-managed
  + Customer-managed : Cloud KMS
  + Customer-supplied

##### [2] Local SSD disks

Physically attached to the VM

+ More IOPS, lower latency, higher throughput than persistent disk
+ 375 GB in size; can attach up to 24 SSD partitions, total of 9TB per instance
+ Data survives a reset, but not a VM stop or terminate
+ VM-specific : CANNOT be reattached to a different VM

##### [3] RAM disk

+ `tmpfs`
+ Fastest type of performance available if you need small data structures
+ Faster than local disk, slower than memory
  + Use when your application expects a file system structure and cannot directly store its data in memory
  + Fast scratch disk, or fast cache
+ Very volatile; erase on stop or restart
+ May need a larger machine type if RAM was sized for the application
+ Consider using a persistent disk to back up RAM disk data

| -                  | Persistent disk **HDD**    | Persistent disk **SSD** | Local SSD disk            | RAM disk                          |
| ------------------ | -------------------------- | ----------------------- | ------------------------- | --------------------------------- |
| Data redundancy    | Yes                        | Yes                     | No                        | No                                |
| Encryption at rest | Yes                        | Yes                     | Yes                       | N/A                               |
| Snapshotting       | Yes                        | Yes                     | No                        | No                                |
| Bootable           | Yes                        | Yes                     | No                        | No                                |
| Use case           | General, bulk file storage | Very random IOPS        | High IOPS and low latency | Low latency and risk of data loss |
|                    | just need capacity         | high performance need   |                           |                                   |


#### Compute pricing

##### Pricing 

+ Per-second billing, with minimum of 1 minute
  + vCPUs, GPUs, and GB of memory
+ Resource-based pricing:
  + Each vCPU and each GB of memory is billed separately
+ Discounts:
  + Sustained use
  + Committed use
  + Preemptible VM instances
+ Recommendation Engine:
  + Notifies you of underutilitzed instances
+ Free usage limits:

#### Special compute configurations

##### Preemptible

+ Lower price for interruptible service (up to 91%)
+ VM might be terminated at any time
  + No charge if terminated in the first minute
  + 24 hours max
  + 30-second terminate warning, but not guaranteed
    + Time for a shutdown script
+ No live migrate; no auto restart
+ You can request that CPU quota for a region be split between regular and preemption
  + Default: preemptible VMs count against region CPU quota

##### Spot VMs

+ the latest version of preemptible VMs
+ Spot VM and preemptible VM share the same pricing model
+ No minimum or maximum runtime
+ Spot VMs are finite Compute Engine Resources, so they might not always be available
+ No live migrate; no auto restart
+ Best practice use cases help you get the most of using Spot VMs

#### Common network designs

##### Increased availability with multiple zones

+ Get improved availability: Place 2 vm into multiple zone but use a single-sub-network

##### Globalization w/ multiple regions

+ Putting resource in different regions: better failure independence

#### Common Comput Engine actions

##### Metadata and scripts

```
# Boot -> Run -> Maintenance -> Shutdown
startup-script-url=URL
shutdown-script-url=URL
```

##### Move an instance to a new zone

```
gcloud compute instance move
```

+ **Automated process** (moving within region)
  + `gcloud compute instances move`
  + Update references to VM; not automatic
+ **Manual process** (moving between regions):
  1. Snapshot all persistent disks on the source VM
  2. Create new persistent disks in destination zone restored from snapshots
  3. Create new VM in the destination zone and attach new persistent disks
  4. Assign static IP to new VM
  5. Update references to VM
  6. Delete the snapshots, original disks, and original VM

---

Quizzes

GCP infrastructure core services - IAM #3

Which of the following is **not** a type of IAM member?

*A. Organization Account*

B. Service Account

C. Google Workspace domain

D. Google Group

E. Cloud identity domain

---

##### Cloud VPN securely connects your on-premises network to your Google Cloud VPC network

+ Useful for <span style="color:yellow">low-volume data connections</span>
+ Classic: 99.9% SLA
+ HA VPN: 99.99% SLA
+ Supports
  + Site-to-site VPN
  + Static routes (Classic VPN only)
  + Dynamic routes (Cloud Router)
  + IKEv1 and IKEv2 ciphers

> In order to create a connection between two VPN gateways, you must establish two VPN tunnels.	

> When using Cloud VPN, the maximum transmission unit (MTU) for your on-premises VPN gateway cannot be greater than 1460 bytes.

##### HA VPN overview

+ Provides 99.99% service availability.

+ Google Cloud automatically chooses two external IP addresses

  + Each of the HA VPNs supports multiple tunnels

    > You can only configure an HA VPN gateway with only ONE active interface and one external IP address.

  + VPN tunnels connected to HA VPN gateways must use dynamic (BGP) routing

+ HA VPN supports site-to-site VPN for different topologies/configuration scenarios:

  + An HA VPN gateway to peer VPN devices
  + An HA VPN gateway to an Amazon Web Services (AWS) virtual private gateway
  + Two HA VPN gateways connected to each other


##### Three typical <span style="color:yellow;">peer gateway</span> configuration for HA VPN:

1. An HA VPN gateway to two separate peer VPN devices, each with its own IP address
2. An HA VPN gateway to one peer VPN device that uses two separate IP addresses 
3. An HA VPN gateway to one peer VPN device that uses one IP address.

##### Use either <span style="color:yellow">transit</span> gateway or a <span style="color:yellow">virtual private</span> gateway, when configuring an HA VPN external VPN gateway to AWS

+ Only the <span style="color:yellow">transit</span> gateway supports equal-cost multipath (ECMP) routing

  > ECMP 等價多路徑路由：
  >
  > To distribute network traffic evenly across **multiple available paths** with the same cost

Cloud VPN supports both static routes and dynamic routes.

You need to configure **Cloud Routers** in order to use dynamic routes.

Cloud Router can manage routes for a Cloud VPN tunnel using **Border Gateway Protocol**.

BGP routing method allows for routes to be updated and exchanged without changing the tunnel configuration.

To set up BGP, an additional IP address has to be assigned to each end of the VPN tunnel. 

+ [HA VPN topologies](https://cloud.google.com/network-connectivity/docs/vpn/concepts/topologies)
+ [Move from Classic VPN to HA VPN](https://cloud.google.com/network-connectivity/docs/vpn/how-to/moving-to-ha-vpn)

> To guarantee **99.99%** availability SLA for HA VPN connections, *properly configure two or four tunnels* from your HA VPN gateway to your peer VPN gateway or to another HA VPN gateway.

##### Use Cloud Interconnect when a dedicated high-speed connection is required between networks

+ Dedicated Interconnect provides a direct connection to a colocation facility.
  + From 10 to 200 Gbps
+ Partner Interconnect provides a connection through a service provider
  + Can purchase less bandwidth from 50 Mbps
+ Allows access to VPC resources using internal IP address space
+ Private Google Access allows on-premises hosts to access Google services using private IPs

---

### Cloud Interconnect and Peering

| *       | Dedicated                              | Shared                          |
| ------- | -------------------------------------- | ------------------------------- |
| Layer 3 | Direct Peering           [<u>Cloud</u> | <u>VPN]</u>     Carrier Peering |
| Layer 2 | Dedicated Interconnect                 | Partner Interconnect            |

#### Dedicated Interconnect provides direct physical connections.

+ 99.9% or 99.99% up-time SLA
+ If your place is no where near any of the colocation facilities locations, consider partner interconnect.

#### Partner Interconnect provides connectivity through a <span style="color:yellow">supported service provider</span>

+ Useful when your data center is not close to colocation facility locations, or if your data needs don't warrant a dedicated interconnect.

##### Comparison of Interconnect options

| Connection             | Provides                                                     | Capacity                         | Requirements                      | Access Type           |
| ---------------------- | ------------------------------------------------------------ | -------------------------------- | --------------------------------- | --------------------- |
| IPsec VPN tunnel       | Encrypted tunnel to VPC networks through the public internet | 1.5-3 Gbps per tunnel            | On-premises VPN gateway           | Internal IP addresses |
| Dedicated Interconnect | Dedicated, direct connection to VPC networks                 | 10 Gbps or 100 Gbps per link     | Connection in colocation facility | Internal IP addresses |
| Partner Interconnect   | Dedicated bandwidth, connection to VPC network through a service provider | 50 Mbps - 10 Gbps per connection | Service provider                  | Internal IP addresses |

##### Direct Peering provides a direct connection between your business network and Google's

+ Broad-reaching edge network locations
+ Exchange BGP routes
+ Reach all of Google's services
+ No SLA
+ Peering requirements

##### Edge Points of Presence (PoPs) 

+ PoPs 邊緣據點 - where Google's network connects to the rest of the Internet via peering
+ 如果你的 data center 還是離 PoPs 很遠，可以考慮 Carrier Peering

##### Carrier Peering provides connectivity through a supported partner

+ Carrier peering partner
+ Reach all of Google's services
+ Partner requirements
+ No SLA

##### Comparision of Peering options

| Connection      | Provides                                                    | Capacity                         | Requirements                    | Access Type         |
| --------------- | ----------------------------------------------------------- | -------------------------------- | ------------------------------- | ------------------- |
| Direct Peering  | Dedicated, direct connection to Google's network            | 10 Gbps per link                 | Connection in Google Cloud PoPs | Public IP addresses |
| Carrier Peering | Peering through service provider to Google's public network | Varies based on partner offering | Service provider                | Public IP addresses |

##### Choosing a network connection option

| interconnect                                                 | peering                                        |
| ------------------------------------------------------------ | ---------------------------------------------- |
| Direct access to RFC1918 IPs in your VPC - with SLA          | Access to Google public IPs only - without SLA |
| 1. Dedicated Interconnect<br />2. Partner Interconnect<br />3. Cloud VPN | 1. Direct Peering<br />2. Carrier Peering      |

##### Managed instance groups

+ Deploy identical instances based on instance template
+ Instance group can be resized
+ Manager ensures all instances are RUNNING
+ Typically used with autoscaler
+ Can be single zone or regional

##### Managed instance groups offer autoscaling capabilities

###### Dynamically add/remove instances:

+ Increases in load
+ Decreases in load

###### Autoscaling policy:

+ CPU utilization
+ Load balancing capacity
+ Monitoring metrics
+ Queue-based workload

### HTTP(S) load balancing

#### HTTP(s) load balancing

+ Global load balancing
+ Anycast IP address
+ HTTP or port 80 or 8080
+ HTTPs on port 443
+ IPv4 or IPv6
+ Autoscaling
+ URL maps

#### HTTP(s) load balancing

+ Target HTTP(S) proxy
+ One signed SSL certificate installed (minimum)
+ Client SSL session terminates at the load balancer
+ Support the QUIC transport layer protocol

#### Network endpoint groups (NEGs)

A network endpoint group (NEG) is a configuration object that specifies a group of backend endpoints or services.

There are four types of NEGs:

+ Zonal
+ Internet
+ Hybrid connectivity
+ Serverless

### Cloud CDN

> content delivery network
>
> Why? Cloud CDN caches contents. You can enable it whiling setting up backend service load balancer.

#### Cloud CDN cache modes

+ Cache modes control the factor that determine whether or not Cloud CDN caches your content.
+ Cloud CDN offers three cache modes:
  1. <span style="color:skyblue">USER_ORIGIN_HEADERS</span>
     + Requires origin responses to set valid cache directives and valid caching header
  2. <span style="color:skyblue">CACHE_ALL_STATIC</span>
     + Automatically caches static content that doesn't have the no-store, private or no-cache directive
     + Origin responses that set valid caching directives are also cached.
  3. <span style="color:skyblue">FORCE_CACHE_ALL</span>
     + Unconditionally caches responses, overriding any cache directives set by the origin.
     + Careful not to caches private per-user content if using a share backend with this mode configured.

#### SSL proxy load balancing

+ Global load balancing for encrypted, non-HTTP traffic
+ Terminates SSL session at load balancing layer
+ IPv4 or IPv6 clients
+ Benefits
  + Intelligent routing
  + Certificate management
  + Security patching
  + SSL policies

#### Network load balancing

##### Network load balancing

+ Regional, non-proxide load balancer
+ Forwarding rules (IP protocol data)
+ Traffic
  + UDP
  + TCP/SSL ports
+ Architecture
  + Backend service-based
  + Target pool-based

##### Backend service-based architecture

+ Regional backend service
+ Defines the behavior of the load balancer and how it distributes traffic to its backend instance groups
+ Backend services enable new features not supported with legacy target pools
  + Non-legacy health checks
  + Auto-scaling with managed instance groups
  + Connection draining
  + Configurable failover policy

##### Target pool-based architecture

+ Forwarding rules (TCP and UDP)
+ Each project can have up to 50 target pools
+ Each target pool can only have one health check
+ All instances in a target pool must be in the same region
  (same limitation as the load balancer)

#### Internal load balancing

##### Internal TCP/UDP load balancing

+ Regional, private load balancing
  + VM instances in same region
  + RFC 1918 IP address
+ TCP/UDP traffic
+ Reduced latency, simpler configuration
+ Software-defined, fully distribute load balancing

> Andromeda: directly deliver client traffic to backend instance

#### Internal HTTP(s) load balancing

+ Regional, private loading balancing
  + VM instances in same region
  + RFC 1918 IP addresses
+ HTTP, HTTPS, or HTTP/2 protocols
+ Based on open source Envoy proxy

#### Choosing a load balancer

+ Support for IPv6: only HTTPS, SSL proxy and TCP proxy support IPv6.

---

1. Google Cloud console:

   Recommanded when you are new to using a service or if you prefer UI

2. Cloud Shell:

   When you are confortable using a specific service, or

   you want to quickly create resources using the command line


---

#### Terraform

##### Infrastructure as code(IaC) allows quick provisioning and removing of infractures

+ Build an infrastructure when needed.
+ Destroy the infrastructure when not in use.
+ Create identical infrastructures for dev, test, and prod.
+ Can be part of a CI/CD pipeline
+ Templates are the building blocks for disaster recovery procedures
+ Manage resource dependencies and complexity
+ Google Cloud supports many IaC tools
  + Terraform, Ansible, Chef, Packer, puppet


##### Terraform is an infrastructure automation tool

+ Repeatable deployment process
+ *Declarative language*
  + HashCorp Language (HCL)

+ Focus on the application
+ Parallel deployment
+ Template-driven

##### Terraform language

+ Terraform language is the interface to declare resources

+ Resources are infrastructure objects (containers, compute engine, etc)

+ The configuration file guides the management of the resource

  + Blocks: Represent objects
  + Argument: to assign values to names

  ```
  resource "google_compute_network" "default" {
    name = "${var.network_name}"
    auto_create_subnetworks = false
  }
  
  <BLOCK TYPE> "<BLOCK LABEL" "<BLOCK LABEL>" {
    # Block body
    <IDENTIFIER> = <EXPRESSION> # Argument
  }
  ```

##### Terraform can be used on multiple public and private clouds

+ Considered a first-class tool in Google Cloud
+ Already installed in Cloud Shell

```tf
provider "google" {
  region       = "us-central"
}
resource "google_compute_instance" {
  name         = "instance name"
  machine_type = "n1-standard-1"
  zone         = "us-central1-f"
  
  disk {
     image = "image to build instance"  
  }
}

output "instance_ip" {
  value = "${google_compute.ip_address}"  
}
```

##### Deploying infrastructure with Terraform

```terminal
terraform init
# to initialize the new Terraform configuratio
# make sure that the Google provider plug-in is downloaded and installed in a subdirectory of the current working directory along with various other bookkeeping files

terraform plan
# to perfrom a refresh unless explicitly disabled and then determines what actions are necessary to achieve the desired state specified in the configuration files
# convenient for checking whether the execution plan for a set of changes matches your expectations, without making any changes to real resources or to the state

terraform apply
# to create the infrastructure defined in the main.tf file
# once completed, you will be able to access the defined infrastructure
```

#### Google Cloud Marketplace

##### Google Cloud Marketplace

+ Deploy production-grade solutions
+ Single bill for Google Cloud and third party services
+ Manage solutions using Terraform
+ Notifications when a security updated is available
+ Direct access to partner support

#### Managed Services

##### BigQuery

###### BigQuery is Google Cloud's serverless, highly scalable and cost-effective cloud data  warehouse

+ Fully managed
+ Petabyte scale
+ SQL interface
+ Very fast

You can access BigQuery by using the <span style="color:yellow">cloud console</span>, by using a <span style="color:yellow">command line tool</span>, or by <span style="color: yellow">making calls to the BigQuery rest API</span> using a variety of client libraries such as Java, .net, or Python.

###### Query example

```sql
WITH groceries AS
  (SELECT "milk" AS dairy,
   "egg" AS protein,
   "bread" AS grain)
SELECT g.*
FROM groceries AS g;

+-------+---------+-------+
| dairy | protein | grain |
+-------+---------+-------+
| milk  | eggs    | bread |
+-------+---------+-------+
```

##### Dataflow

###### Use Dataflow to execute <span style="color:skyblue">a wide variety of data processing patterns</span>

+ Serverless, fully managed data processing

+ Batch and stream processing with autoscale

+ Open source programming using `beam`

  > Cloud Dataflow supports fast, simplified pipeline development via expressive SQL, Java and Python APIs in the Apache Beam SDK.

+ Intelligently scale to millions of QPS

##### Dataprep

###### Use Dataprep to visually explore, clean and prepare data for analysis and machine learning

+ Serverless, works at any scale
  + no infrastructure to deploy or manage
+ Suggests ideal data transformation
+ Focus on data analysis (skip data-profiling)
+ Integrated partner service operated by Trifecta

##### Dataproc

###### Dataproc is a service for running Apache Spark and Apache Hadoop clusters

+ Low cost (per-second billing, preemptible)

+ Super fast to start, scale, and shut down

  + 90 secs vs 5~30 mins

+ Integrated with Google Cloud, e.g.

  (BigQuery, Cloud Storage, Cloud Bigtable, Stackdriver Logging, Stackdriver Monitoring)

  + Provides complete data platform rather than just a Spark/Hadoop cluster

+ Managed service

+ Simple and familiar

##### Dataflow versus Dataproc

```
          [   Dependencies on specific   ]
   +------[ tools/packages in the Apache ]-----+
   |      [   Hadoop/Spark ecosystem?    ]     |
 [Yes]                                       [No]  
   |																						|
   |                          [     Do you prefer the manual or     ]
   |                     +--- [ automatic provisioning of clusters? ] ---+
   |                     |                                               |
   |                 [Manual]                                       [Automatic]
   |                     |                                               |
   ↓                     |                                               ↓ 
[Dataproc] ← ------------+                                          [Dataflow]
```

> Serverless:
>
> Servers or Compute Engine instances are obfuscated so that you don't have to worry about the infrastructure.

---

## Design and Process

Microservices:

+ Allow a large application to be decomposed into independent constituent parts with each part having its own area of responsibility
+ To serve a single user or API requests, a microservice's base application can call many internal microservices to compose its response

#### Requirements, Analysis and Design

##### Qualitative requirements define systems from the user's point of view

| Ws   | Examples                                                     |
| ---- | ------------------------------------------------------------ |
| Who  | Who are the users?<br />Who are the developers?<br />Who are the stakeholders? |
| What | What does the system do?<br />What are the main features?    |
| Why  | Why is the system needed? ⭐️⭐️⭐️                                |
| When | When do the users need and/or want the solution?<br />When can the developers be done? |
| How  | How will the system work?<br />How many users will there be?<br />How much data will there be? |

##### Roles represent the goal of a user at some point

| Roles are not people or job titles                           | Roles should describe a users objective                      | Examples of Roles                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. People can play multiple roles<br />2. A single role can be played by multiple people | 1. What does the user want to do?<br />2. "User" is not a good role (everyone is a user) | 1. Shopper<br />2. Account holder<br />3. Customer<br />4. Adminstrator<br />5. Manager |

Using persona can provide further insights.

##### Evaluate user stories with the INVEST criteria

1. **I**ndepedent
2. **N**egotiable
3. **V**aluable
4. **E**stimatable
5. **S**mall
6. **T**estable

##### Quantitative requirements are things that are measurable

Given the contraints:

+ Time
+ Finance
+ People

What can be achieved:

+ How many users are there
+ How much data is there
+ What are the rewards and risk


---

#### DevOps Automation

##### Continuous intregration pipelines automate building applications

1. <span style="color:skyblue">Developers check-in code</span>

   Use a Git repo for each microservice and branches for versions.

2. <span style="color:skyblue">Run unit tests</span>

   If the tests don't pass, stop.

3. <span style="color:skyblue">Build deployment package</span>

   Create a Docker image.

4. <span style="color:skyblue">Deploy</span>

   Save your new Docker image in a container registry.

##### Google provides the components required for a continuous integration pipeline

+ **Continuous Integration**

  + **Cloud Source Repositories**

    Developers push to a central repository when they want a build to occur

  + **Cloud Build**

    Build system executes the steps required to make a deployment package or Docker image

  + **Build triggers**

    Watches for changes in the Git repo and starts the build

  + **Container Registry**

    Store your Docker images or deployment packages in a central location for deployment

##### Cloud Source Repositories provides managed Git repositories

Control access to your repos using IAM within your Google Cloud projects.

##### Cloud Build lets you build software quickly across all languages

+ Google-hosted Docker build services

  + Alternative to using Docker build command

+ Use the CLI to submit a build

  ```terminal
  gcloud builds submit --tag gcr .io/your-project-id/image-name
  ```

##### Build triggers watch a repository and build a container whenever code is pushed

+ Supports Maven, custom builds, and Docker

  [Select source] --> [Select repository] --> [Trigger settings]

##### Container Registry is a Google Cloud-hosted Docker repository

+ Images built using Cloud Build are automatically saved in Container Registry

  + Tag images with the prefix **gcr.io/your-project-id**/image-name

+ Can use Docker push and pull commands with Container Registry

  ```terminal
  docker push gcr.io/your-project-id/image-name
  docker pull gcr.io/your-project-id/image-name
  ```

##### Binary authorization allows you to enforce deploying only trusted containers into GKE

+ Enable binary authorization on GKE cluster
+ Add a policy that requires signed images
+ When an image is built by Cloud Build an "attestor" verifies that it was from a trusted repository (Source Repositories, for example)
+ Container Registry includes a vulnerability scanner that scans containers

#### Infrastructure as Code

##### Moving to the cloud requires a mindset change

###### On-Premises

+ Buy machines
+ Keep machines running for years
+ Prefer fewer big machines
+ Machines are capital expenditures

###### Cloud

+ Rent machines
+ Turn machines off as soon as possible
+ Prefer lots of small machines
+ Machines are monthly expenses

##### In the cloud, all infrastructure needs to be disposable

+ Don't fix broken machines
+ Don't install patches
+ Don't upgrade machines
+ If you need to fix a machine, delete it and re-create a new one
+ To make infrastructure disposable, automate everything with code:
  + Can automate using scripts
  + Can use declarative tools to define infrastructure

> //TODO: Move section Terraform here

#### Key Storage Characteristics

##### Google Cloud storage and database portfolio

| Relational                                            | Relational                                   | NoSQL                                    | NoSQL                                     | Object                                        | Warehouse                                | In memory                                  |
| ----------------------------------------------------- | -------------------------------------------- | ---------------------------------------- | ----------------------------------------- | --------------------------------------------- | ---------------------------------------- | ------------------------------------------ |
| Cloud SQL                                             | Cloud Spanner                                | Fi**r**estore                            | Cloud Bigtable                            | Cloud Storage                                 | BigQuery                                 | Memorystore                                |
| Good for:<br />Web frameworks                         | Good for:<br />RDBMS+scale, HA, HTAP         | Good for:<br />Hierarchical, mobile, web | Good for:<br />Heavy read + write, events | Good for: <br />Binary object data            | Good for:<br />Enterprise data warehouse | Good for:<br />Caching for Web/Mobile apps |
| Such as:<br />CMS, eCommerce                          | Such as:<br />User metadata, Ad/Fin/MarTech  | Such as:<br />User profiles, Game State  | Such as:<br />AdTech, financial, IoT      | Such as:<br />Images, media serving,  backups | Such as:<br />Analytics, dashboards      | Such as:<br />Game state, user sessions    |
| Scales to 30 TB.  <br />MySQL, PostgreSQL, SQL Server | Scales infinitely Regional or multi-regional | Completely managed Document database     | Scales infinitely Wide-column NoSQL       | Completely managed Infinitely scalable        | Completely Managed SQL analysis          | Managed Redis DB                           |
| Fixed schema                                          | Fixed schema                                 | Schemaless                               | Schemaless                                | Schemaless                                    | Fixed schema                             | Schemaless                                 |

```java
if (data.isStructured()) {
    if (data.isAnalyticWorkload()) {
        if (data.requiresUpdateOrLowLatency()) {
            return CLOUD_BIGTABLE;
        } else {
            // Updates not in need
            return BIG_QUERY;
        }
    } else {
        // Non-analytic workload
        if (data.isRelational()) {
            if (data.isHorizontalScalabilityNeeded()) {
                return CLOUD_SPANNER;
            } else {
                return CLOUD_SQL;
            }
        } else {
            // Note: don't confuse this with Filestore!
            return FIRESTORE;
        }
    }
} else {
    // Non-structured data
    if (data.isSharedFileSystemNeeded()) {
        return FILESTORE;
    } else {
        return CLOUD_STORAGE;
    }
}
```



##### Different data storage services have different availability SLAs

| Storage Choice                      | Availability SLA % |
| ----------------------------------- | ------------------ |
| Cloud Storage (multi-region bucket) | >= 99.95           |
| Cloud Storage (regional bucket)     | 99.9               |
| Cloud Storage (coldline)            | 99.0               |
| Spanner (multi-region)              | 99.999             |
| Spanner (single region)             | 99.99              |
| Firestore (multi-region)            | 99.999             |
| Firestore (single region)           | 99.99              |

##### Durability represents the odds of losing data

Preventing data loss is a shared responsibility

| Storage Choice | Google Cloud Provides                                        | What you should do       |
| -------------- | ------------------------------------------------------------ | ------------------------ |
| Cloud Storage  | 11 9's durability Versioning (optional)                      | Turn versioning on       |
| Disks          | Snapshots                                                    | Schedule snapshot jobs   |
| Cloud SQL      | Automated machine backups<br />Point-in-time recovery<br />Failover server (optional) | Run SQL database backups |
| Spanner        | Automatic replication                                        | Run export jobs          |
| Firestore      | Automatic replication                                        | Run export jobs          |

##### The amount of data and number of read and writes is important when selecting a data storage servic

1. Scale horizontally by adding nodes
   + Bigtable
   + Spanner
2. Scale vertically by making machine larger
   + Cloud SQL
   + Memorystore
3. Scale automatically with NO limits
   + Cloud Storage
   + BigQuery
   + Firestore

##### Do you need strong consistency?

Strongly consistent databases update all copies of data within a transaction.

Ensures everyone gets the latest copy of the data on reads:

+ Storage
+ Cloud SQL
+ Spanner
+ Firestore

Eventually consistent databases update one copy of the data and the rest asynchronously.

Can handle a large volume of writes:

+ Bigtable
+ Memorystore replicas 

##### Calculate the total cost per GB when choosing a storage service

+ Bigtable and Spanner would be too expensive for storing smaller amounts of data

+ Firestore is less expensive per GB, but you also pay for reads and writes

+ Cloud Storage is relatively cheap, but you can't run a database in storage

+ BigQuery storage is relatively cheap, but doesn't provide fast access to records and you have to pay for running queries

  > You need to choose the right storage solutions for each of your microservices based on their requirements

---

#### Choosing Google Cloud Storage and Data Solutions

##### Storage Transfer Service

###### Import online data to Cloud Storage

+ Amazon S3
+ HTTP/HTTPS Location
+ Transfer data between Cloud Storage buckets

###### Scheduled jobs

+ One time or recurring, import at a scheduled time of day
+ Options for delete objects not in source or after transfer
+ Filter on file name, creation date

##### Use the Storage Transfer Service for on-premises data for large-scale uploads from your data center

+ Install on-premises agent on your servers

+ Agent runs in a Docker container

+ Set up a connection to Google  Cloud

+ Requires a minimum of <span style="color:yellow">300 Mbps</span> bandwidth

  > To use the STS for on-premises:
  >
  > 1. A Posix-compliance source
  > 2. At least 300 megabits per second network connection
  > 3. A Docker supported Linux server, ports 80 and 443 open for outbound connections

+ Scales to billions of files and 100s of TBs

+ Secure

+ Automatic retires

+ Logged

+ Easy to monitor via the Cloud Console

##### Transfer Appliance

Suitable for data upto 1PB; rackable device up to 1PB shipped to Google

Use Transfer Appliance if uploading your data would take too long

AES256 encrypted the moment of capture ; data erased per NSIT 800-88 standard

Shipped in tamper-evident seals

Steps:

1. Request Transfer Appliance

2. Encrypt and copy your data

3. Ship it back to Google

4. Google loads the data

5. You decrypt your data 

   (You control the encryption key.)

---

#### Designing Google Cloud Networks

##### In Google Cloud, VPC networks are global

+ When creating networks, create subnets for the regions you want to operate in
+ Resources across regions can reach each other without any added interconnect
+ If you are a global company, choose regions around the world
+ If your users are close together, choose the region closest to them plus a backup region
+ A project can have multiple networks

##### When creating custom subnets, specify the region and the internal IP address range

+ IP address ranges **cannot overlap**
+ Machines in the same VPC can communicate via their internal IP address regardless of the subnet region
+ Subnets don't need to be derived from a single CIDR block
+ Subnets are expandable without down time
+ IP Aliasing or Secondary range can be set on the subnet

##### A single VM can have multiple network interfaces connecting to different networks

+ Each network must have a subnet in the region the VM is created in
+ Each interface must be attached to a different VPC
+ Maximum of 8 interfaces per VM

##### A Shared VPC is created in one project, but can be shared and used by other projects

Requires an organization

+ Create the VPC in the host project
+ Share VPC adin shares the VPC with other service projects

Allows centralized control over network configuration

+ Network admins configure subnets, firewall rules, routes, etc.
+ Remove network admin rights from developers
+ Developers focus on machine creation and configuration in the shared network
+ Disable the creation of the default network using an organizational policy

#### Designing Google Cloud Load Balancers

##### Use a global load balancer to provide access to services deployed in multiple regions

+ Global load balancing supported by HTTP load balancer and TCP and SSL proxies
+ HTTP load balancer routes requests to the region closest to the user
  + Uses a global, anycast IP address 

##### Use a regional load balancer to provide access to services deployed in a single region

+ Supported by HTTP, TCP and UDP load balancers
+ Can have a public or private IP address
+ Can use any TCP or UDP port

##### If your load balancers have public IPs, secure them using SSL

+ Supported by HTTP and TCP load balancers
+ Self-managed and Google-managed SSL certificates

##### For lower-latency and decreased egress cost leverage Cloud CDN

+ Can be enable when configuring the HTTP global load balancer
+ Caches static content worldwide using Google Cloud edge-caching locations
+ Cache static data from web servers in Compute Engine instances, GKE pods, or Cloud Storage Buckets

---

#### Connecting Networks

1. Peering
2. Cloud VPN
3. Cloud Interconnect

##### Use VPC peering to connect networks when they are both in Google Cloud

+ Can be the same or different organizations
+ Subnet ranges cannot overlap
+ Network admins for each VPC must approve the peering requests

```
+-Organization---------+         	  	+-Organization---------+
| Google Cloud Project |              | Google Cloud Project |
| +-----------------+  |              | +-----------------+  |  
| | VPC             |  |              | | VPC             |  |  
| | +-------------+ |  | VPC Peering  | | +-------------+ |  |
| | | Subnet      |-----------------------| Subnet      | |  |
| | | 10.1.1.0/24 | |  |              | | | 10.1.2.0/24 | |  | 
| | +-------------+ |  |              | | +-------------+ |  | 
| +-----------------+  |              | +-----------------+  | 
+----------------------+              +----------------------+ 
```

##### Cloud VPN securely connects your on-premises network to your Google

---

##### When designing for reliability, consider these key performance metrics

+ Availability (可用性 - 系統可接收請求的時間%)

  The percent of time a system is running and able to process requests

  + Achieved with fault tolerance
  + Create backup systems
  + Use health checks
  + Use clear box metrics to count real traffic success and failure

+ Durability (因為硬體系統錯誤而遺失資料的可能性)

  The odds of losing data because of a hardware or system failure

  + Achieved by replicating data in multiple zones
  + Do regular backups
  + Practice restoring from backups

+ Scalability (可擴張性)

  The ability of a system to continue to work as user load and data grow

  + Monitor usage
  + Use capacity auto-scaling to add and remove servers in response to changes in load

#### Designing for Reliability

##### Avoid single points of failure

A spare spare, N+2

+ Define your unit of deployment
+ N+2L: Plan to have one unit out for upgrade or testing and survive another failing
+ Make sure that each unit can handle the extra load
+ Don't make any single unit too large
+ Try to make units interchangeable stateless clones

##### Beware of correlated failures

Correlated failures occur when related items fail at the same time

+ If a single machine fails, all requests served by machine fail
+ If a top-of-rack switch fails, entire rack fails
+ If a zone or region is lost, all the resources in it fail
+ Servers on the same software run into the same issue
+ If a global configuration system fails, and multiple systems depend on it, they potentially fail too

The group of related items that could fail together is a **failure domain**

##### To avoid correlated failures...

Decouple servers and use microservices distributed among multiple failure domains

+ Divide business logic into services based on failure domains
+ Deploy to multiple zones and/or regions
+ Split responsibility into components and spread over multiple processes
+ Design independent, loosely coupled but collaborating services

##### Beware of cascading failures

Cascading failures occur when one system fails, causing others to be overloaded, such as a message queue becoming overloaded because of a failing backend

```
                       +-[600]->[Server A, max 1000 qps]
                       |
[Cloud Load Balancing]-+-[600]->[Server B, max 1000 qps]

# Server B fails, causing A to be overloaded [1200->A]
```

##### To avoid cascading failures

+ Use health checks in Compute Engine or readiness and liveliness probes in Kubernetes to detect and then repair unhealthy instances
+ Ensure that new server instances start fast and ideally don't rely on other backends/systems to start up

```
                       +-[300]->[Server A, max 500 qps]
                       |
                       +-[300]->[Server B, max 500 qps]
                       |
[Cloud Load Balancing]-+-[300]->[Server C, max 500 qps]
                       |
                       +-[300]->[Server D, max 500 qps]

# Server D fails, sending [400] respectively to Server A/B/C
```

##### Plan against Query of death overload

+ Where a request made to a service causes a failure in the service
+ Reason: The error manifests itself as overconsumption of resource, but in reality is due to an error in business logic itself (overloads of service)
+ Solution: To monitor query performance. Ensure that notification of these issues gets back to the developers

##### Plan against Positive feedback cycle overload failure

+ Where a problem is caused by trying to prevent problems
+ Problem: You try to make the system more reliable by adding retries, and instead you create the potential for an overload
+ Solution: Prevent overload by carefully considering overload conditions whenever you are trying to improve reliability with feedback mechanisms to invoke retries (*use intelligent retries* - two strategies)

##### Use <span style="color:yellow">(1) truncated exponential backoff pattern</span> to avoid positive feedback overload at the client

+ If service invocation fails, try again:
  + Continue to retry, but wait a while between attempts
  + Wait a little longer each time the request fails
  + Set a maximum length of time and a maximum number of requests
  + Eventually, give up
+ Example:
  + Request fails: wait 1 sec + *random_number_milliseconds*  and retry
  + Req fails: wait 2 sec + *random_number_milliseconds*  and retry
  + Req fails: wait 4 sec + *random_number_milliseconds*  and retry
  + And so on, up to a *maximum_backoff*  time
  + Continue waiting and retrying up to some maximum number of retries

##### Use the <span style="color:yellow"> (2) circuit breaker pattern</span> to protect the service from too many retries

+ Plan for degraded state operations
+ If a service is down and all it's clients are retrying, the increasing number of requests can make matters worse
  + Protect the service behind a proxy that monitors service health (*the circuit breaker*)
  + If the service is not healthy, don't forward requests to it
+ If using GKE, leverage istio to automatically implement circuit breakers

##### Use lazy deletion to reliably recover when users delete data by mistake

---

#### Disaster Planning

##### High availability can be achieved by deploying to multiple zones in a region

+ Deploy multiple servers
+ Orchestrate servers with a regional managed instance group
+ Create a failover database in another zone, or use a distributed database like Firestore or Spanner

##### Kubernetes clusters can also be deployed to single or multiple zones

+ Kubernetes cluster consist of a collection of node pools
+ Selecting Regional location type replicates node pools in multiple zones in the region specified

##### Create a health check when creating instance gorups to enable auto healing

+ Create a test endpoint in your service
+ Test endpoint needs to verify that the service is up, and also that it can communicate with dependent backend database and services
+ If health check fails, the instance group will create a new server and delete the broken one
+ Load balancers also use health checks to ensure that they send requests only to healthy instances

##### If using Cloud SQL, create a failover replica for high availability

+ Replica will be created in another zone in the same region as the database
+ Will automatically switch to the failover if the primary instance is unavailable
+ Doubles the cost of the database

##### Disaster recovery: Cold standby

+ Create snapshots, machine images, and data backups in multi-region storage
+ If main region fails, spin up servers in backup region
+ Route requests to new region
+ Document and test recovery procedure regularly

##### Disaster recovery: Hot standby

+ Create instance groups in multiple regions
+ Use a global load balancer
+ Store unstructured data in multi-region buckets
+ For structured data, use a multi-region database such as Spanner or Firestone

##### When disaster planning, brainstorm scenarios that might cause data loss and/or service failure

+ What could happen that would cause a failure?
+ What is the Recovery Point Objective (amount of data that would be acceptable to lose)?
+ What is the Recovery Time Objective (amount of time it can take to be back up and running)?

| Service                | Scenario                                    | Recovery Point Objective | Recovery Time Objective | Priority |
| ---------------------- | ------------------------------------------- | ------------------------ | ----------------------- | -------- |
| Product Rating Service | Programmer deleted all ratings accidentally | 24 hours                 | 1 hour                  | Med      |
| Orders service         | Database server crashed                     | 0                        | 1 minutes               | High     |

##### Based on your disaster scenarios, formulate a plan to recover

+ Devise a backup strategy based on risk and recovery point and time objectives
+ Communicate the procedure for recovering from failures
+ Test and validate the procedure for recovering from failures regularly
+ Ideally, recovery becomes a streamlined process, part of daily operations

| Resource                | Backup Strategy         | Backup Location                     | Recovery Procedure                                           |
| ----------------------- | ----------------------- | ----------------------------------- | ------------------------------------------------------------ |
| Ratings MySQL database  | Daily automated backups | Multi-regional Cloud Storage bucket | Run Restore Script                                           |
| Orders Spanner database | Multi-region deployment | us-east1 backup region              | Snapshot and backup at regular intervals, outside of the serving infrastructure; e.g. Cloud Storage |

---

### Network Security and Encryption

#### Network Security

##### Remove external IPs to prevent access to machines outside their network

+ Use a bastion host to provide access to private machines

+ Can also SSH into internal machines using Identity-Aware Proxy from the console and CLI

+ Use Cloud NAT to provide egress to the internet from internal machines

  > *All internal traffic should terminate at a load balancer, third-party firewall(proxy or WAF) API Gateway, or IAP. That way, internal services cannot be launched and get public IP addresses.*

##### Private access allows access to Google Cloud services using an internal address

+ Enabled when creating subnets
+ Allow access to Google Cloud services from VMs that only have internal IPs
  + For example, a machine with only an internal IP would be able to reach a Cloud Storage bucket

```terminal
gcloud compute networks subnets 
update subnet-b \ 
    --enable-private-ip-google-access
```

##### Configure firewall rules to allow access to VMs

+ By default, ingress on all ports is denied
+ Add firewall rules to control which clients have access to which VMs on which ports
+ Application level security is the responsibility of the customer

##### Control access to APIs using <span style="color:skyblue">Cloud Endpoints</span>

+ Protect and monitor your public APIs
+ Control who has access to your API
+ Validate every call with JSON Web Tokens and Google API keys
+ Integrates with Identity Platform

##### Restrict access to your services to TLS only

+ All Google Cloud service endpoints use HTTPS
+ It's up to you to configure your service endpoints
+ In the load balancer setup, only create a secure frontend

##### Leverage Google Cloud network services for DDoS protection

+ Global load balancers detect attacks and drop them
+ Enabling the CDN will protect backend resources

##### Use <span style="color: skyblue">Google Cloud Armor</span> to create network security policies

+ Can allow or deny access to your Google Cloud resources using IP addresses or ranges
+ Create allow lists to allow known addresses
+ Create deny lists to block known attackers

##### Cloud Armor supports layer 7 web application firewall (WAF) rules

+ Predefined rules for preventing common attacks like SQL injection and cross-site scripting

+ Flexible rules language allows you to allow or deny traffic using request headers, geographic location, ip addresses, cookies, etc.

+ Examples:

  ```console
  inIpRange(origin.ip, '9.9.9.0/24')
  request.headers['cookie'].contains('80=BLAH')
  origin.region_code == 'AU'
  inIpRange(origin.ip, '1.2.3.4/32') &&
  request.headers['user-agent'].contains('WordPress')
  evaluatePreconfiguredExpr('xss-canary')
  ```

#### Encryption

##### Google Cloud provides server-side encryption of data at rest by default

+ Data Encryption Key (DEK) uses AES-256 symmetric key
+ Keys are encrypted by Key Encryption Keys (KEK)
+ Google controls root keys in Cloud KMS
+ Keys are automatically periodically rotated
+ On-the-fly decryption by authorized user access with no visible performance impact

##### For compliance reasons, you may need to manage your own keys

+ Customer-managed encryption keys are created in the cloud using Cloud Key Management Service(KMS)
+ You create the keys and specify the rotation frequency
+ You can then select your keys when creating storage resources like bucket and disks

##### Customer-supplied encryption keys are created in your environment and provided to Google Cloud

+ Use your own keys with Google Cloud services
+ CSEK are supplied by the calling application per-API call
+ Only cached in RAM by Google
+ They decrypt a single payload (or column) or block of returned data
+ Supported by Compute Engine (persistent disks) and Cloud Storage

##### The Data Loss Prevention API can be used to protect sensitive data by finding it and redacting it

+ Scans data in Cloud Storage, BigQuery, or Firestore
+ Can also scan images
+ Detects many different types of sensitive data, including:
  + Emails
  + Credit cards
  + Tax IDs
+ You can add your own information types
+ Cloud DLP API can delete, mask, tokenize, or just identify the location of the sensitive data

---

### Manage Versions and Cost Planning

#### Managing Versions

##### In a microservice architecture, be careful not to break clients when services are updated

+ Include version in URI
  + If you deploy a breaking change, you need to change the version
+ Need to deploy new versions with zero downtime
+ Need to effectively test versions prior to going live

##### Rolling updates allow you to deploy new versions with no downtime

+ Typicall, you have multiple instances of a service behind a load balancer
+ Update each instance one at a time
+ Rolling updates work when it is ok to have 2 different versions running simultaneously during the update
+ Rolling updates are a feature of instance groups; just change the instance template
+ Rolling updates are the default in Kubernetes; just change the Docker image
+ Completely automated in App Engine

##### Use a blue/green deployment when you don't want multiple versions of a service running simultaneously

> When you want to test a new software version → deploy it to the GREEN environment
>
> Once testing complete, the workload is shifted from the current, i.e. BLUE env to the GREEN env.

+ In Compute Engine, you can use **DNS** to migrate requests from one load balancer to another
+ In Kubernetes, configure your service to **route to the new pods using labels**
  + Simple configuration change
+ In App Engine, use the **Traffic Splitting** feature

##### Canary releases can be used prior to a rolling update to reduce the risk

+ The current service version continues to run
+ Deploy an instance of the new version and give it a portion of requests
+ Monitor for errors
+ In Compute Engine, you can create a new instance group and add it as an additional backend in your load balancer
+ In Kubernetes, create a new pod with the same labels as the existing pods; the service will automatically route a portion of requests to it
+ In App Engine, use the Traffic Splitting feature



#### Cost Planning

##### Capacity planning is a continuous, iterative cycle

[Continuous Integration : F -> A -> A -> D -> F ... ]

1. **Forecast**

   Estimate capacity needed Monitor Repeat

2. **Allocate**

   Determine resources required to meet forecasted capacity

3. **Approve**

   Cost estimation versus risks and rewards

4. **Deploy**
   Monitor to see how accurate your forecasts were

##### Optimizing cost of compute

+ Start with small VMs, and test to see whether they work
+ Consider more small machines with auto scalng turned on
+ Consider committed use discounts
+ Consider at least some preemptible instances:
  + 80% discount
  + Use auto healing to recreate VMs when they are preempted
+ Google Cloud rightsizing recommendations will alert you when VMs are underutilized

##### Optimizing disk cost

+ Don't over-allocate disk space
+ Determine what performance characteristics your applications require:
  + I/O Pattern: small reads and writes or large reads and writes
  + Configure your instances to optimize storage performance
+ Depending on I/O requirements, consider Standard over SSD disks

##### To optimize network costs, keep machines close to your data

+ Egress in the same zone is free
+ Egress to a different Google Cloud service within the same region using an external IP address or an internal IP address is free
  + Except for some services such as Memorystore for Redis
+ Egress between zones in the same region is charged
+ All internet egress is charged

##### <span style="color:skyblue">GKE usage metering</span> can prevent over-provisioning Kubernetes clusters

+ Agent collects consumption metrics in addition to the resource requests by polling PodMetrics objects from the metrics server
+ The <u>resource request records</u> and <u>resource consumption records</u> are exported to two separate tables in a BigQuery dataset that you specify
+ Comparing requested with consumed resources makes it easy to spot waste and take corrective measures

##### Consider alternative services to save cost rather than allocating more resources

+ CDN -
  for static content
+ Caching - 
  Memorystore as a cache
+ Messaging / Queuing - 
  + Instead of using datastore between two apps, use messaging or queuing with Pub/Sub to decouple communicating services and reduce storage needs
+ etc.

##### Use the Google Cloud Pricing Calculator to estimate costs

+ Base your cost estimates on your forecasting and capacity planning

+ Compare the costs of different compute and storage services

  [Link to the GCPricingCalculator](https://cloud.google.com/products/calculator)

##### Billing reports provide detailed cost breakdowns

##### For advanced cost analysis, export billing data to BigQuery

##### Visualize spend with Google Data Studio

Billing Dashboard:

+ Daily View / Monthy View / Overall

##### Set budgets and alerts to keep your team aware of how much they are spending

+ The alerts send emails to Billing Admins after spend exceeds a percent of the budget
+ In addition to receiving an email, you can use Pub/Sub notifications to programmatically receive spend updates about this budget.
+ You can even create a Cloud Function that listens to the Pub/Sub topic to automate cost management 

---

### Monitoring Dashboards

#### Monitoring Dashboard

##### Google Cloud unifies the tools you need to monitor your service SLOs and SLAs

Monitoring, Logging, Trace, Debugger, Error Reporting, Profiler

##### Monitoring dashboards monitor your services

+ Monitor the things you pay for:
  + CPU use
  + Storage capacity
  + Reads and writes
  + Network egress
  + etc.
+ Monitor your SLIs to determine whether you are meeting your SLOs

---

## k8s

#### Cloud Computing and Google Cloud

##### Cloud computing - 5 fundamental attributes

1. On-demand self-service

   No human intervention needed to get resources

2. Broad network access
   Access from anywhere

3. Resource pooling
   Provider shares resources to customers

4. Rapid elasticity
   Get more resources quickly as needed

5. Measured service
   Pay only for what you consume

##### Google Cloud offers a range of services

1. Compute Engine
   + run VM on demand
   + maximum flexibility
2. GKE
   + run containerization application
   + package code, highly portable
3. App Engine
   + platform as service
   + resources managed by Google Cloud
4. Cloud Function
   + only pay for the service while your code runs 

##### Managed service

###### Storage

1. Cloud Bigtable
2. Cloud Storage
3. Cloud SQL
4. Cloud Spanner
5. Datastore

###### Big Data

1. BigQuery
2. Pub/Sub
3. Dataflow
4. Dataproc
5. Notebooks

###### Machine Learning

1. Vision API
2. Vertex AI
3. Speech-to-Text API
4. Cloud Translation API
5. Cloud Natural Language API

---



### Resource Management

##### Multi-Region

1. the Americas

2. Asia-Pacific

3. Europe

   + **Region** : e.g. `europe-west2`

     + **Zone** : e.g.

       `europe-west2-a`

       `europe-west2-b`

       `europe-west2-c`

##### PoPs and network

+ designed to provide the highest possible throughput, lowest possible latencies

#### Quotas

##### All resources are subject to project quotas or limits

+ How many resources you can create per project
  + 15 VPC networks/project
+ How quickly you can make API requests in a project: rate limits
+ 5 admin actions/sec (Cloud Spanner)
+ How many resources you can create per region
  + 24 CPUs region/project

##### Why use project quotas?

+ Prevent runaway consumption in case of an error or malicious attack
+ Prevent billing spikes or surprises
+ Forces sizing consideration and periodic review

#### Labels

##### Labels are a utility for organizing Google Cloud resources

+ Attached to resources : VM, disk, snapshot, image
  + Google Cloud console, `gcloud`, or API
+ Example uses of labels:
  + Inventory
  + Filter resources
  + In scripts
    + Help analyze costs
    + Run bulk operations

##### Use labels for

+ Team or Cost Center

  ```
  team:marketing
  team:research
  ```

+ Components

  ```
  component:redis
  component:frontend
  ```

+ Environment or stage

  ```
  environment:prod
  environment:test
  ```

+ Owner or contact

  ```
  owner:steve
  contact:hon
  ```

+ State

  ```
  state:inuse
  state:readyfordeletion
  ```

#### Billing

##### How billing works

+ Billing account pays for project resources
+ A billing account is linked to one or more projects
+ Charged automatically or invoiced every month or at threshold limit
+ Subaccounts can be used for separate billing for projects

##### How to keep your billing under control

+ Budgets and alerts 
  +  to set up a web hook, or to trigger shut down script
+ Billing export
  + to send billing data to BigQuery dataset
+ Reports

##### Quotas are helpful limits

1. Rate Quota
   + Reset after an amount of time
   + Example: 1,000 requests per 100 seconds
2. Allocation Quota
   + Govern the resource you can have in your projects
   + Example: 5 networks per project

Many quotas are changeable - by requesting an increase from Google Cloud support

### Interacting with Google Cloud

1. Google Cloud Console

   Web user interface (GUI)

2. Cloud SDK and Cloud Shell

   Command-line interface 

   + `gcloud` `kubectl` `gsutil` `bq`
   + Cloud Shell Editor

3. Cloud Console mobile app

   For iOS and Android

   + Start, stop and use SSH to connect to Compute Engine instances
   + Billing info and alert
   + Set up customized stuff

4. REST-based API

   For custom applications

### Containers and Container Images

#### Introduction to Containers

##### We used to build apps on individual servers

Deployment ~months, low utilization, not portable

```
+--------------------+
| Dedicated server   |
| +----------------+ |
| |Application Code| |
| +----------------+ |
| +----------------+ |
| |  Dependencies  | |
| +----------------+ |
| +----------------+ |
| |     Kernel     | |
| +----------------+ |
| +----------------+ |
| |    Hardware    | |
| +----------------+ |
+--------------------+
```

##### Hypervisors create and manage virtual machines

+ Hypervisor 
  + the software layer that breaks the dependencies of an operating system with its underlying hardware, and allow several virtual machines to share that same hardware. 
  + e.g. KVM
+ Deployment ~days (mins), improved utilization, Hypervisor-specific
  + Dependencies and OS are still bundled together
  + Not very easy to move from a VM from one hypervisor product to another
  + Applications with shared dependencies are NOT isolated from each other
  + The resources requirements from one application can **starve out** other applications of the resources that they need (搶奪資源)
  + A dependency upgrade for one app might cause another app to stop working

```
+--------------------+
| Dedicated server   |
| +----------------+ |
| |Application Code| |
| +----------------+ |
| +----------------+ |
| |  Dependencies  | |
| +----------------+ |
| +----------------+ |
| |     Kernel     | |
| +----------------+ |
| +----------------+ |
| |   Hardware +   | | 
| |   Hypervisor   | | 
| +----------------+ |
+--------------------+
```

##### The VM-centric way to solve problems 

(Resource-starved-out and Dependency-upgrade)

+ Each application maintains its own dependencies
+ The kernel is isolated



#### Containers and Container Images

##### Why developers like containers

+ Image: application and its dependencies
+ Docker: build and run container images
+ Dockerfile: Docker formatted container image
  + Each instruction in the Dockerfile specifies a layer inside the container image. Each layer is read only
  + When a container runs from this image, it will also have a <span style="color:skyblue">writable ephemeral top most layer</span>

+ When you write your Dockerfile:
  + Organize from layer least likely to change through, to layers most likely to change

```dockerfile
FROM  // starts out by creating a base layer pulled from a public repo
COPY  // adds a new layer containing some files copied in from your build tools' current directory
RUN   // builds your app using the make command and posts the results of the build into the 3rd layer

// Last layer specifies what command to run within the container when it's launched
ENV
EXPOSE 
CMD
```

#### Intro to Kubernetes

##### What is Kubernetes?

1. Open source 
   + container-centric management environment
2. Automation
3. Container management
4. Declarative configuration
   + Describe a desired state you want to achieve, instead of command lines
5. Imperative configuration
   + For quick temporary fixes

##### Kubernetes features

1. Supports both stateful and stateless applications
2. Autoscaling
3. Resource limits
4. Extensibility

#### Intro to Google K8s Engine

Kubernetes is powerful, but managing the infrastructure is a full-time job.

Google Kubernetes Engines to the rescue! It helps to deploy, manage and scale K8S environment for your containerized application

##### Explaining GKE features

+ Fully managed
+ Container-optimized OS
+ Auto upgrade
+ Auto repair
+ Cluster scaling
+ Seamless integration
+ Identity and Access Management
+ Integrated logging and monitoring (integrated w/Stackdriver)
+ Integrated networking
+ Cloud Console

#### Computing Options Detail

##### Compute Engine

+ Fully customizable virtual machines
+ Persistent disks and optional local SSDs
+ Global load balancing and autoscaling
+ Per-second billing

##### Compute Engine use cases

1. Complete control over the OS and virtual hardware
2. Well suited for lift-and-shift migrations to the cloud
3. Most flexible compute solution, often used when a managed solution is too restrictive

##### App Engine

+ Provides a fully managed, code-first platform
+ Streamlines application deployment and scalability
+ Provides support for popular programming languages and application runtimes
+ Supports integrated monitoring, logging and diagnostics
+ Simplifies version control, canary testing, and rollbacks

##### App Engine use cases

1. Websites
2. Mobile app and gaming backends
3. RESTful APIs

##### Google Kubernetes Engine

+ Fully managed Kubernetes platform
+ Supports cluster scaling, persistent disks, automated upgrades, and auto node repairs
+ Built-in integration with Google Cloud services
  + Cloud Build, Container Registry, Stackdriver Monitoring, Stackdriver Logging
+ Portability across multiple environments
  + Hybrid computing
  + Multi-cloud computing

##### GKE use cases

1. Containerized applications
2. Cloud-native distributed systems
3. Hybrid application

##### Cloud Run

+ Enables stateless containers
  + Build, deploy and manage modern serverless workloads
+ Abstracts away infrastructure management
+ Automatically scales up and down
+ Never have to pay for over-provisioned resources
  + only charged for the resources you use calculated down to the nearest 100 milliseconds
+ Open API and runtime environment

##### Cloud Run use cases

1. Deploy stateless containers that listen for requests or events
2. Build applications in any language using any frameworks and tools

##### Cloud Functions

+ Event-driven, serverless compute service
+ Automatic scaling with highly available and fault-tolerant design
+ Charges apply only when your code runs
  + Each function, invocation, memory, CPU use is measured in the 100 milliseconds increments rounded
  + Cloud Functions also provides a perpetual free tier
+ Triggered based on events in Google Cloud services, HTTP endpoints, and Firebase

##### Cloud Functions use cases

1. Supporting microservice architecture
2. Serverless application backends
   + Mobile and IoT backends
   + Integrate with third-party services and APIs
3. Intelligent applications
   + Virtual assistant and chat bots
   + Video and image analysis

### K8s Concepts

##### There are two elements to Kubernetes objects

+ Kubernetes objects 
  + Persistent entities representing the state of the cluster
+ Object spec
  + Desired state described by us
+ Object status
  + Current state described by Kubernetes

##### Containers in a Pod share resources

A pod embodies the environment where containers live and that environment can accommodate one or more containers.

If there's more than one container in a Pod, they are tightly coupled and they share resources including networking and storage.

```
+---------------+ +-------------------------------------------+
| Pod           | | Pod                                       |
|               | | +---------------------------------------+ | 
|               | | |           Shared networking           | |
|               | | +---------------------------------------+ | 
| +-----------+ | | +-----------+ +-----------+ +-----------+ | 
| | Container | | | | Container | | Container | | Container | | 
| +-----------+ | | +-----------+ +-----------+ +-----------+ | 
|               | | +---------------------------------------+ | 
|               | | |           Shared storage              | |
|               | | +---------------------------------------+ | 
+---------------+ +-------------------------------------------+
```

#### The K8S Control Plane

##### Cooperating processes make a Kubernetes cluster work

+ Cluster

  + Control plane

    + **kube-APIserver**: 

      any query or changes towards the cluster's state must be addressed to the cube-APIserver

      1. **etcd**: the cluster's database, to reliably store the state of the cluster

         + all of the cluster configuration data

         + more dynamic information
           + what nodes are part of the cluster
           + what pods should be running and where they should be running

      2. **kube-scheduler**:

         + Responsible for scheduling pods onto nodes

         + It discovers a pod object that doesn't yet have an assignment to a node, it chooses a node and simply writes the name of that node into the pod object

      3. **kube-controller-manager**

         + continuously monitor the state of the cluster through kube-apiserver

      4. **kub-cloud-manager**

         + manages controllers that interact with the underlying cloud providers

         + if you manually launch a K8S cluster on Google Compute Engine, kube-cloud-manager would be responsible for bringing in Google Cloud features like load balancers and storage volumes when you need them

         + Each node runs a small family of control plan components. e.g. a kubelet

           > **<span style="color:skyblue">Kubelet</span>** : 
           >
           > + Kubernetes agent on each node
           > + When the kube-apiserver wants to start a pod on a node, it connects to that nodes kubelet

### GKE Concepts

`kubeadm`: An open source command that can automate much of the initial set up of a cluster

##### GKE manages all the control plane components

+ GKE still exposes an IP address to which we send all of our K8S API requests
+ GKE takes charge of managing all of the control plane infrastructure behind it

##### GKE : More about nodes

❌ Kubernetes doesn't create nodes. Cluster admins create nodes and add them to K8S

✅ GKE manages this by *deploying and registering Compute Engine instances as nodes*



##### Use node pools to manage different kinds of nodes

+ Node Pool : 
  + A subset of nodes within a cluster that share a configuration, such as their amount of memory or their CPU generation
  + Also provide an easy way to ensure that workloads run on the right hardware within your cluster
  + Node pools are a **GKE feature** rather than a K8S feature

##### Zonal versus regional clusters

1. Zonal Cluster

   + By default, a cluster launches on a single Google Cloud Compute Zone with 3 identical nodes, all in one node pool

   + The number of nodes can be changed during or after the creation of cluster

   + Adding more nodes and deploying multiple replicas of an application will improve an app's availability, but only up to a point

2. Region Cluster

   + A regional cluster is spread across 3 zones, each containing 1 control plane and 3 nodes
   + A zonal cluster cannot be converted to regional cluster

##### A regional or zonal GKE cluster can also be set up as a private cluser

+ Hidden from the public internet
+ Cluster control plances can be accessed 
  + by GC products through an **internal IP address**
  + By authorized networks through **external IP address**

### K8s Object Management

> Incomplete :( 

### Migrate for Anthos

> Incomplete :(

---

### App Engine

With App Engine, you can choose from popular coding languages, libraries, and frameworks to develop apps with tools you're familiar with. 

Then automatically provision servers in scale app instances based on demand. 

This means you can upload your code and Google will manage your apps availability. 

Coding options include Eclipse, IntelliJ, Maven, Git, Jenkins, and PyCharm. 

With App Engine, there are no servers to provision or maintain. 

App Engine provides built-in services and APIs like NoSQL data stores, memcache, load balancing, health checks, application logging and a user authentication API that's common to most applications. 

App Engine also offer software development kits or SDKs to help you develop, deploy, and manage your apps on your local machine. 

Each SDK includes all of the APIs and libraries available to App Engine, the simulated secure sandbox environment that emulates all of the App Engine services on your local computer and deployment tools to upload your application to the Cloud and manage different versions. 

The SDK manages your application locally, and the Google Cloud Console manages your application in production. 

You can use the Cloud Console web-based interface to create new applications, configure domain names, change which version of your application is live, examine access and error logs and much more. 

From a security perspective, the Security Command Center, Google Cloud security and risk management platform, keeps web applications safe. 

Through the Cloud Console, you can use the Security Command Center to automatically scan and detect common web application vulnerabilities.

#### App Engine environment



##### Flexible environment

+ Instances are health-checked, healed, and co-located 
+ Critical, backward-compatible updates are automatically applied to the underlying operating system
+ VM instances are automatically located by geographical region according to the settings in your project
+ VM instances are restarted on a weekly basis

##### The flexible environment supports ...

1. Microservices
2. Authorization
3. SQL & NoSQL databases
4. Traffic splitting
5. Logging
6. Search
7. Versioning
8. Security scanning
9. Memcache
10. CDN

##### App Engine <span style="color: skyblue">flexible</span> allows users to ...

1. Benefit from custom configurations and libraries, while focusing on writing code
2. Customize the runtime and the operating system of your virtual machine. 
   Standard runtimes include Python, Java, Go, Node.js, PHP, .NET, and Ruby
3. Customize or provide runtimes by supplying a custom Docker image or Dockerfile

##### Standard vs Flexible

| -                              | Standard environment                                         | Flexible environment                                        |
| ------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| Instance startup               | Seconds <span style="color:skyblue">fast</span>              | Minutes                                                     |
| SSH access                     | No                                                           | Yes (although not by default)                               |
| Write to local disk            | No (some runtimes have read and write access to the `/tmp` directory) | Yes, ephemeral (disk initialized on each VM startup)        |
| Support for 3rd party binaries | For certain languages                                        | Yes                                                         |
| Network access                 | Via App Engine services                                      | Yes                                                         |
| Pricing model                  | After free tier usage, pay per instance class, with automatic shutdown | Pay for resource allocation per hour; no automatic shutdown |

##### App Engine vs GKE ...

+ **App Engine standard environment** is for people who want the service to <span style="color:yellow">take maximum control of their web and mobile applications deployment and scaling</span>
+ **Google Kubernetes Engine** - gives the application owner the full flexibility of Kubernetes
+ **App Engine flexible environment** - somewhere between the two

#### Google Cloud API management tools

+ Cloud Endpoints
  1. Distributed API management system
  2. Provides an API console, hosting, logging, monitoring and other features
  3. Use with any APIs that support the OpenAPI Specification
  4. Supports applications running in App Engine, Google Kubernetes Engine and Compute Engine
  5. Clients include Android, iOS and Javascript
+ API Gateway
  1. Backend implementation can vary for a single service provider
  2. Provide secure access to your backend services through a well-defined REST API
  3. Client consume your REST APIS to implement standalone apps
+ Apigee API management

---

#### Cloud Run

+ A managed compute platform that can run stateless containers
+ Serverless, removing the need for infrastracture management
+ Built on Knative, an open API and runtime environment built on Kubernetes
+ Can automatically scale up and down from zero almost instantaneously, charging only for the resources used

##### Three step:

1. Write your code
2. Build and package
3. Deploy to Cloud Run

> Cloud Run then starts your container on demand to handle requests, and ensures that all incoming requests are handled by <span style="color:skyblue;">dynamically adding and removing</span> containers



#### Development in the cloud

##### Cloud Source Repositories

+ provide full-featured git repositories hosted on Google Cloud that support the collaborative development of any application or service, including those that run on App Engine and Compute Engine
+ capable of having any number of private git repositories
+ **Diagnostic Tools** : debugger, error reporting
+ Can be migrated from github or gitlab

##### Cloud Function

+ Lightweight, event-based, asynchronous compute solution 
+ Allows you to create small, single-purpose functions that respond to cloud events without the need to manage a server or a runtime environment
+ Use these functions to construct applications from bite-sized business logic and connect and extend cloud services
+ Billed to the nearest 100 milliseconds, and only while your code is running
+ Supports writing source code in a number of programming languages, including Node.js, Python, Go, Java, .Net Core, Ruby, and PHP
+ Events from Cloud Storage and Pub/Sub can trigger Cloud Functions asynchronously, or use HTTP invocation  for synchronous execution

#### Deployment : Infrastructure as Code


Creating an environment in Google Cloud can mean lots of work like setting up a compute network and storage resources and then keeping track of their configurations. 

This process can be done manually by writing the commands you need to set up your environment the way you want. 

However, this is labor-intensive and requires updating commands if you want to change the environment or manually writing new commands if you want to clone an environment. 

It's more efficient to use a template. 

Using a template allows you to write the specification to your application environment in the same way you'd write a configuration file. 

A Go template can then be deployed in a scout environment to quickly create as many identical 

application environments as needed. 

##### Terraform:

1. Create a template file using **HashiCorp Configuration Language (HCL)** that describes what the components of the environment should look like
2. Terraform uses that template to determine the actions needed to create the environment your template describes
3. Use Terraform to update the environment to match the change
4. Store and version-control Terraform templates in Cloud Source Repositories

---

### Virtual Private Cloud

#### VPC

You can provision your GCP resources, connect them to each other, and isolate them from each other in a virtual private cloud.

You can also define fine-grained network and policies within GCP and between GCP and on-premises or other public Clouds.

##### VPC objects

+ Projects
+ Networks
  + default, auto mode, custom mode
+ Subnetworks - divide or segregate your environment
+ Regions
+ Zones
+ IP addresses
  + Internal, external, ranges
+ Virtual machines
+ Routes
+ Fire rules

#### Projects, networks, subnetworks

###### A project:

+ Associates objects and services with billing
+ Contains networks (up to 15) that can be shared/peered

###### A network:

+ Has no IP address range
+ Is global and spans all available regions (spreads across multi-regions)
+ Contains subnetworks (regional)
+ Is available as default, auto, or custom

##### 3 VPC network types

###### Default

+ Every project
+ One subnet per region
+ Default firewall rules

###### Auto Mode

+ Default network
+ One subnet per region
+ Regional IP allocation
+ Fixed /20 mask subnetwork per region
+ Expandable up to /16 mask
  + all of these subnets fit within the `10.128.0.0/9` CIDR block

###### Custom Mode

+ No default subnets created
+ Full control of IP ranges
+ Regional IP allocation 
+ Expandable to IP ranges you specify
  + These IP ranges cannot overlap between subnets of the same network
  + Custom mode network cannot be changed to auto mode network (one-way)

##### Subnetworks cross zones

+ VMs can be on the same subnet but in different zones
+ A single firewall rule can apply to both VMs even though they are in different zones

> 1. Every subnet has four reserved IP addresses in its primary IP range
> 2. Because a region contains several zones, subnetworks can cross zones

##### Expand subnets without re-creating instances

+ The new subnet must NOT overlap with other subnets in the same VPC network in any region
+ IP range must be a unique valid CIDR block
+ New subnet IP ranges have to fall within valid IP ranges
+ Can expand but not shrink
+ Auto mode can be expanded from /20 to /16
+ Avoid creating overly large subnets
  + Do not scale your subnet beyond what you actually need

#### IP addresses

##### VMs can have internal and external IP addresses

###### Internal IP

+ Allocated from subnet range to VMs by DHCP (dynamic host configuration protocol)
+ DHCP lease is renewed every 24 hours
+ VM name + IP is registered with netowork-scoped DNS

###### External IP

+ Assigned from pool (ephemeral)
+ Reserved (static)
+ Bring Your Own IP address (BYOIP)
+ VM doesn't know external IP; it is mapped to the internal IP

#### Mapping IP addresses

##### External IPs are mapped to internal IPs

...

##### DNS resolution for internal addresses

Each instance has a hostname that can be resolved to an internal IP address:

+ The hostname is the same as the instance name
+ FQDN is [hostname].[zone].c.[project-id].internal
  + Example: `my-server.us-central1-a.c.guestbook-151617.internal`

Name resolution is handled by internal DNS resolver:

+ Provided as part of Compute Engine (169.254.169.254)
+ Configured for use on instance via DHCP
+ Provides answer for internal and external addresses

##### DNS resolution for external addresses

+ Instances with external IP addresses can allow connections from hosts outside the project

  + Users connect directly using external IP address

  + Admins can also publish public DNS records pointing to the instance

    + Public DNS records are not published automatically,

      but admins can publish these using existing DNS servers.

+ DNS records for external addresses can be published using existing DNS servers (outside of Google Cloud)

+ DNS zones can be hosted using Cloud DNS

##### Host DNS zones using Cloud DNS

Cloud DNS is a scalable, reliable and managed authoritive Domain Name System

+ Google's DNS service

+ Cloud DNS translate domain names into IP address 

  Example: `www.google.com` → `74.125.29.101`

+ Low latency

+ High availability (100% uptime SLA)

+ Create and update millions of DNS records

+ UI, command line, or API

##### Alias IP ranges 

> Assign a range of IP addresses as aliases to a VM's network interface using alias IP ranges

#### Routes & firewall rules

##### A route is a mapping of an IP range to a destination

Every network has:

+ Routes that let instances in a network send traffic directly to each other
+ A default route that directs packets to destinations that are outside the network

**Firewall rules must also allow the packet.**

##### Routes map traffic to destination networks

<span style="color:skyblue">A route is created when a network is created</span>, enabling traffic delivery from "anywhere".

Also <span style="color:skyblue">a route is created when a subnet is created</span>, this is what enables VMs on the same network to communicate.

+ Apply to traffic egressing a VM
+ Forward traffic to most specific route
+ Are created when a subnet is created
+ Enable VMs on same network to communicate
+ Destination is in CIDR notation
+ Traffic is delivered only if it also matches a firewall rule

##### Firewall rules protect your VM instances from unapproved connections

+ VPC network functions as a distributed firewall
+ Firewall rules are applied to the network as a whole
+ Connections are allowed or denied at the instance level
+ Firewall rules are stateful
  + If a connection is allowed between a source and a target or a target at a destination, all subsequent traffic in either direction will be allowed
  + In other words, firewall rules allow bidirectional communication once a session is established
+ Implied deny all incress and allow all egress (if all firewall rules in a network are deleted)

##### A firewall rule is composed of...

| Parameter               | Details                                                      |
| ----------------------- | ------------------------------------------------------------ |
| `direction`             | Inbound connections are matched against **ingress** rules only.  Outbound connections are matched against **egress** rules only. |
| `source or destination` | For the `ingress` direction, `sources` can be specified as part of the rule with IP addresses, source tags or a source service account.  For the `egress` direction, `destinations` can be specified as part of the rule with one or more ranges of IP addresses. |
| `protocol` and `port`   | Any rule can be restricted to apply to specific protocols only or specific combinations of protocols and ports only. |
| `acion`                 | To allow or deny packets that match the direction, protocol, port and source or destination of the rule |
| `priority`              | Governs the order in which rules are evaluated; the first matching rule is applied |
| `Rule assignment`       | All rules are assigned to all instances, but you can assign certain rules to certain instances only |

##### Google Cloud firewall use case : <span style="color:skyblue">Egress</span>

Conditions:

+ Destination CIDR ranges
+ Protocols
+ Ports

Action:

+ Allow: permit the matching egress connection
+ Deny: block the matching egress connection

##### Google Cloud firewall use case : <span style="color:skyblue">Ingress</span>

Conditions:

+ Source CIDR ranges
+ Protocols
+ Ports

Action:

+ Allow: permit the matching ingress connection
+ Deny: block the matching ingress connection

#### Pricing

##### Network pricing (subject to change)

| Traffic type                                                 | Price            |
| ------------------------------------------------------------ | ---------------- |
| ingress                                                      | No charge        |
| egress to the same zone (internal IP address)                | No charge        |
| egress to Google products (YouTube, Maps, Drive)             | No charge        |
| egress to a different Google Cloud service (within same region; exceptions) | No charge        |
| egress between zones in the same region (per GB)             | $0.01            |
| egress to the same zone (external IP address, per GB)        | $0.01            |
| egress between regions within the US and Canada (per GB)     | $0.01            |
| Egress between regions, not including traffic between US regions | Varies by region |

---

### Common network designs

#### Common network designs

##### Cloud NAT

+ Google's manged network address translation service
+ let you provision your application instances without public IP address, while also allowing them to access the internet in a controlled and efficient manner
+ Outbound NAT:
  + Enable two private isntances to access an update server on the Internet
+ Cloud NAT does NOT implement inbound NAT
  + Hosts outside your VPC network cannot directly access any of the private instances behind the cloud NAT gateway
  + This helps you keep your VPC network isolated and secure

---

### SLOs, SLIs, and SLAs

#### KPIs and SLIs

##### Key performance indicators (KPIs) are metrics that can be used to measure success

###### In business, common KPIs include:

+ Return on investment (ROI)
+ Earnings before interest and taxes (EBIT)
+ Employee turnover
+ Customer churn

###### In software, common KPIs include:

+ Page views
+ User registrations
+ Clickthroughs
+ Checkouts

##### For KPIs to be effective, they must be SMART

+ Specific
  "User Friendly" is not as specific as "Section 508 Accessible"
+ Measurable
  You have to find an obejctive way to test whether you're meeting your KPIs
+ Achievable:
  "100% Availability" might sound good, but it's not really possible
+ Relevant:
  Does it really matter to the user? Will it help achieve application goals?
+ Time-bound:
  99% available: Per year? Per month? Per day? If we don't know, how can we measure?

#### SLOs and SLAs

1. SLI (indicators)
   + An SLI is a measurable attribute of a service. A KPI
   + Example: Availability
2. SLO (objectives)
   + The SLO is the number or the goal you want to achieve for a given SLI for a given duration.
   + Do you want 95%, 99%, or 99.99% availability?
3. Agreements (agreements)
   + An SLA is a binding contract providing the customer compensation if the service doesn't meet specific expectations
   + The SLA is a more restrictive version of the SLO.

##### SLIs must be time-bound and measurable

❌ Fast response time

✅ HTTP GET requests respond within 400 ms aggregated per minutes

❌ Highly available

✅ Percentage of successful requests over all requests aggregated per minute

##### SLOs must be achievable and relevant

| SLO                                                          | SLO (%) | SLO                                                          |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| HTTP POST photo uploads complete within 100ms aggregated per minute | 99%     | ❌ If our users are using mobile phones, maybe this is overkill |
| 🔝                                                            | 80%     | ✅ This might be good enough                                  |
| Available as measured with an uptime check every 10 seconds aggregated per minute | 100%    | ❌ Sounds good, but not practical                             |
| 🔝                                                            | 99.999% | ❌ Possible, but maybe too expensive                          |
| 🔝                                                            | 99%     | ✅ Maybe good enough and easier and more cost-effective       |

##### Tips for determining SLOs

+ The goal isn't to make SLOs as high as possible; the goal is to make them as low as you can get away with, while still making users happy. That's why it's important to understand your users
+ The higher you set the SLO, the higher the cost in compute resources (redundancy) and operations effort (people time)
+ Applications should not significantly outperform their SLOs, because users come to expect the level of reliability you usually give them

##### An SLA is a business contract between the provider and the customer

The SLA stipulates that:

+ A penalty will apply to the provider if the service doesn't maintain certain availability and/or performance thresholds
+ If the SLA is broken, the customer will receive compensation from the provider

Not all services have an SLA, but all services should have an **SLO**

Your SLO thresholds should be stricter than your SLA

##### Example:

+ SLI : The latency of successful HTTP responses (HTTP-200)
+ SLO : The latency of 99% of the responses must be ≤ 200 ms (較嚴格)
+ SLA : The user is compensated if 99th percentile latency exceeds 300 ms (較寬鬆)

##### Examples

| User Story                | SLO                                           | SLI                                                          |
| ------------------------- | --------------------------------------------- | ------------------------------------------------------------ |
| Search Hotel and Flight   | Available 99.95%                              | Fraction of 200 vs 500 HTTP responses from API endpint measured per month |
| Search Hotel and Flight   | 95% of requests will complete in under 200 ms | Time to last byte GET requests measured every 15 seconds aggregated per 5 minutes |
| Supply Hotel Inventory    | Error rate of < 0.00001%                      | Upload errors measured as a percentage of bulk uploads per day by custom metric |
| Supply Hotel Inventory    | Available 99.9%                               | Fraction of 200 vs 500 HTTP responses from API endpoint measured per month |
| Analyze sales performance | 95% of queries will complete in under 10s     | Time to last byte GET requests measured every 60 seconds aggregated per 10 minutes |

---

### Microservices

#### Microservices

##### Microservices divide a large program into multiple smaller, independent services

+ Monolithic applications implement all features in a single code base with a database for all data
+ Microservices have multiple code bases, and each service manages its own data
  + Each services have their own databases

##### Pros and cons of microservice architectures

###### [Pros]

1. Easier to develop and maintain
2. Reduced risk when deploying new versions
3. Service scale independently to optimize use of infrastructure
4. Faster to innovate and add new features
5. Can use different languages and frameworks for different services
6. Choose the runtime appropriate to each service

###### [Cons]

1. Increased complexity when communicating between services
2. Increased latency across service boundaries
3. Concerns about securing inter-service traffic
4. Multiple deployments
5. Need to ensure that you don't break clients as versions change
6. Must maintian backward compatibility with clients as the microservice evolves

##### The key to architecting microservice applications is recognizing service boundaries

| <span style="color:yellow">I. Decompose app by feature to minimize dependencies</span> | <span style="color:lightgreen">II. Organize services by architectural layer</span> | <span style="color:skyblue">III. Isolate services that provide shared functionality</span> |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. Reviews service<br />2. Orders service<br />3. Products service<br />4. Etc. | 1. Web, Android, and iOS user interfaces<br />2. Data access services | 1. Authentication service<br />2. Reporting service<br />3. Etc. |

##### Stateful services have different challenges than stateless ones

Stateful services manage stored data over time

+ Harder to scale
+ Harder to upgrade
+ Need to back up

Stateless services get their data from the environment or other stateful services

+ Easy to scale by adding instances
+ Easy to migrate to new versions
+ Easy to administer

##### Avoid storing shared state in-memory on your servers

+ Requires sticky sessions(session affinity) to be set up in the load balancer
+ Hinders elastic autoscaling

##### Store state using backend storage services shared by the frontend server

+ Cache state data for faster access
+ Take advantage of Google Cloud-managed data services
  + Firestore, Cloud SQL, etc. for state
  + Memorystore for Redis for caching

#### Microservices Best Practices

##### The Twelve-Factor App is a set of best practices for building web or software-as a-service applications

> Helps to decouple components of the application

+ Maximize portability
+ Deploy to the cloud
+ Enable continuous deployment
+ Scale easily

##### The 12 factors

1. **Codebase**

   One codebase tracked in revision control, many deploys

   + Use a version control system like Git

   + Each app has one code repo and vice versa

     > Cloud source repository

2. **Dependencies**
   Explicitly declare and isolate dependencies

   + Use a package manager like Maven, Pip, NPM to install dependencies
   + Declare dependencies in your code base

   > Container Registry (for storing image)

3. **Config**

   Store config in the environment

   + Don't put secrets, connection strings, endpoints, etc., in source code
   + Store those as environment variables

4. **Backing Services**

   Treat backing services as attached resources

   + Databases, caches, queues, and other services are accessed via URLs
   + Should be easy to swap one implementation for another

5. **Build, release, run**
   Strictly separate build and run stages

   1. Build creates a deployment package from the source code
   2. Release combines the deployment with configuration in the runtime environment
   3. Run executes the application

6. **Processes**
   Execute the app as one or more stateless processes

   + Apps run in one or more processes
   + Each instance of the app gets its data from a separate database service

7. **Port binding**

   Export services via port binding

   + Apps are self-contained and expose a port and protocol internally
   + Apps are not injected into a separate server like Apache

   > Such apps can be deployed on platform services such as Compute Engine, GKE, App Engine, or Cloud Run

8. **Concurrency**

   Scale out via the process model

   + Because apps are self-contained and run in separate process, they scale easily by adding instances

9. **Disposability**

   Maximize robustness with fast startup and graceful shutdown

   + App instances should scale quickly when needed
   + If an instance is not needed, you should be able to turn it off with no side effects

10. **Dev/prod parity**

    Keep development, staging, and production as similar as possible

    + Container systems like Docker makes this easier
    + Leverage infrastructure as code to make environments easy to create

    > To build workflow & keep the environments consistent:
    >     Cloud Source Repositories
    >     Cloud Storage
    >     Container Registry
    >     Terraform

11. **Logs**

    Treat logs as event streams

    + Write log messages to standard output and aggregate all logs to a single source

    > To help with collection, processing, and structured analysis of log: 
    >     Stackdriver Logging
    >     Cloud Logging
    >     Cloud Logging Log-based Metrics
    >
    > ​    Cloud Pub/Sub
    > ​    BigQuery
    > ​    Cloud Dataflow

12. **Admin processes**

    Run admin/management tasks as one-off processes

    + Admin tasks should be repeatable processes, not one-off manual tasks
    + Admin tasks shouldn't be a part of the application

    > Many options depending on your deployment on Google Cloud:
    >
    > ​    Cron jobs in GKE
    >
    > ​    Cloud tasks on App Engine
    >
    > ​    Cloud Scheduler

### REST and APIs

#### REST

##### A good microservice design is <span style="color:yellow">loosely coupled</span>

+ Clients shouldn't need to know too many details of services they use

+ Services communicate via HTTPS using text-based payloads

  + Client makes GET, POST, PUT or DELETE request
  + Body of the request is formatted as JSON or XML
  + Results returned as JSON, XML, or HTML

+ Services should add functionality without breaking existing clients

  + Add, but don't remove, items from responses

  > *If microservices aren't loosely coupled, you'll end up with a really complicated monolith*

##### REST architecture supports loose coupling

+ REST stands for Representational State Transfer
+ Protocol independent
  + HTTP is most common
  + Others possible like gRPC
+ Service endpoints supporting REST are called RESTful
+ Client and Server communicate with Request - Response processing

##### RESTful services communicate over the web using HTTP(S)

+ URIs (or endpoints) identify resources
  + Responses return an immutable reprentation of the resource information
+ REST applications provide consistent, uniform interfaces
  + Representation can have links to additional resources
+ Caching of immutable representations is appropriate

##### Resources and representations

+ Resource is an abstract notion of information
+ Representation is a copy of the resource information
  + Representations can be single items or a collection of items

##### Passing representations between services is done using standard text-based formats

+ JSON, HTML, XML, CSV

#### HTTP

##### Clients access services using HTTP requests

+ VERB: GET, PUT, POST, DELETE
+ URI: Uniform Resource Identifier (endpoint)
+ Request Header: metadata about the message
  + Preferred representation formats (e.g. JSON, XML)
+ Request Body: (Optional) Request state
  + Representation (JSON, XML) of resource

##### HTTP requests are simple and text-based

```
GET / HTTP/1.1
Host: jingle.ysatshei.com
```

```
POST /add HTTP/1.1
Host: jingle.ysatshei.com
Content-Type: json
Content-Length: 35

{"name":"Noir","breed":"Schnoodle"}
```

##### The HTTP verb tells the server what to do

+ `GET` is used to retrieve data
+ `POST` is used to create data
  + Generates entity ID and returns it to the client
+ `PUT` is used to create data or alter existing data
  + Entity ID must be known
  + `PUT` should be idempotent, which means that whether the request is made once or multiple times, the effects on the data are exactly the same

+ `DELETE` is used to remove data

##### Services return HTTP responses

+ Response Code: 3-digit HTTP status code
  + 200 - success
  + 400 codes - client errors
  + 500 codes - server errors
+ Response Body: contains resource representation
  + JSON, XML, HTML, etc.

##### All services need URIs (Uniform Resource Identifiers)

+ Plural nouns for sets (collections)
+ Singular nouns for individual resources
+ Strive for consistent naming
+ URI is case-insensitive
+ Don't use verbs to identify a resource
+ Include version information

#### APIs

##### OpenAPI is an industry standard for exposing APIs to clients

+ Standard interface description format for REST APIs
  + Language independent
  + Open-source (based on Swagger)
+ Allows tools and humans to understand how to use a service without needing its source code

##### gRPC is a lightweight protocol for fast, binary communication between services or devices

+ Developed at Google
  + Supports many languages
  + Easy to implement
+ gRPC is supported by Google services
  + Global load balancer (HTTP/2)
  + Cloud Endpoints
  + Can expose gRPC services using an Envoy Proxy in GKE

##### Google Cloud provides two tools for managing APIs : Cloud Endpoints and Apigee

Both provide tools for:

+ User authentication
+ Monitoring
+ Securing APIs
+ etc.

Both support OpenAPI and gRPC



