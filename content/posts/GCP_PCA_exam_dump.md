---
title: "GCP PCA --- practice exam"
date: 2023-06-25T15:26:29+08:00
author: "celine"
tags: ["GoogleCloudPlatform", "ProfessionalCloudArchitect"]
categories: ["StudyNote"]
---

> 2023-08-04 Google Cloud Platform - Professional Cloud Architect **GET**!  

[VPC firewall rules](https://cloud.google.com/firewall/docs/firewalls)

[Security policy overview](https://cloud.google.com/armor/docs/security-policy-overview)

[Control communication between Pods and Services using network policies](https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy)

[Identity-Aware Proxy overview](https://cloud.google.com/iap/docs/concepts-overview#when_to_use_iap)

[Optimize application latency with load balancing](https://cloud.google.com/load-balancing/docs/tutorials/optimize-app-latency)

[Kubernetes Services](https://cloud.google.com/kubernetes-engine/docs/concepts/service)

[Ingress for internal Application Load Balancers](https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-ilb)

[Connecting to Google Cloud: your networking options explained](https://cloud.google.com/blog/products/networking/google-cloud-network-connectivity-options-explained)

[Carrier Peering --- Considerations](https://cloud.google.com/network-connectivity/docs/carrier-peering#considerations)

[Partner Interconnect overview](https://cloud.google.com/network-connectivity/docs/interconnect/concepts/partner-overview)

[Dedicated Interconnect overview](https://cloud.google.com/network-connectivity/docs/interconnect/concepts/dedicated-overview#before_you_use)

[Overview of VPC Service Controls](https://cloud.google.com/vpc-service-controls/docs/overview)

[Access control lists](https://cloud.google.com/storage/docs/access-control/lists)

[Overview of IAP for on-premises apps](https://cloud.google.com/iap/docs/cloud-iap-for-on-prem-apps-overview)

[SOC 2](https://cloud.google.com/security/compliance/soc-2)

[Using cached query results](https://cloud.google.com/bigquery/docs/cached-results)

[Cloud Audit Logs overview](https://cloud.google.com/logging/docs/audit)

[Cloud Logging overview](https://cloud.google.com/logging/docs/overview)

[Best practices for operating containers](https://cloud.google.com/architecture/best-practices-for-operating-containers)

[VPC Service Controls](https://cloud.google.com/vpc-service-controls)

[VPC Service Controls doesn't support Cloud Shell](https://cloud.google.com/vpc-service-controls/docs/supported-products#shell)

[Bucket locations](https://cloud.google.com/storage/docs/locations#considerations)

[Caching overview --- maximum size](https://cloud.google.com/cdn/docs/caching#maximum-size)

[Cloud CDN --- Hybrid and multi-cloud architectures](https://cloud.google.com/cdn/docs/external-backends-internet-neg-overview#hybrid_and_multi-cloud_architectures)

[Antipattern: Store data greater than 512KB size in cache](https://docs.apigee.com/api-platform/antipatterns/caching-large)

[Set up a multi-cluster mesh on GKE](https://cloud.google.com/service-mesh/docs/unified-install/gke-install-multi-cluster)

[AutoML](https://cloud.google.com/automl)

[De-identifying sensitive data](https://cloud.google.com/automl)

[VPC networks --- IP ranges](https://cloud.google.com/vpc/docs/vpc#ip-ranges)

[Cloud SQL](https://cloud.google.com/sql/)

[Cloud Pub/Sub](https://cloud.google.com/pubsub/)

[Cloud Storage --- Storage classes](https://cloud.google.com/storage/docs/storage-classes)

[Operations Suite, formerly Stackdriver](https://cloud.google.com/products/operations?hl=en)

[BigQuery](https://cloud.google.com/bigquery/)

[Cloud Bigtable](https://cloud.google.com/bigtable/)

[Health checks overview](https://cloud.google.com/load-balancing/docs/health-check-concepts)

[External Application Load Balancer overview](https://cloud.google.com/load-balancing/docs/https/)

[VPC --- Create and manage VPC networks](https://cloud.google.com/vpc/docs/create-modify-vpc-networks)

[VPC --- Routes](https://cloud.google.com/vpc/docs/routes)

[VPC --- Add network tags](https://cloud.google.com/vpc/docs/add-remove-network-tags)

[GKE --- Access secrets stored outside GKE clusters using Workload Identity](https://cloud.google.com/kubernetes-engine/docs/tutorials/workload-identity-secrets)

[Secret Manager](https://cloud.google.com/secret-manager)















## EHR Healthcare

#### Company overview

EHR Healthcare is a leading provider of electronic health record software to the medical industry. EHR Healthcare provides their software as a service to multi-national medical office, hospitals, and insurance providers.

#### Solution concept

Due to rapid changes in the healthcare and insurance industry, EHR Healthcare's business has been growing exponentially year over year. They need to be able to scale their environment, adapt their disaster recovery plan, and roll out new continuous deployment capabilities to update their software at a fast pace. Google Cloud has been chosen to replace their current colocation facilities.

#### Existing technical environment

EHR's software is currently hosted in multiple colocation facilities. The lease on one of the data centers is about to expire. 

Customer-facing applications are web-based, and many have recently been containerized to run on a group of Kubernetes clusters. Data is stored in a mixture of relational and NoSQL databases (MySQL, MS SQL Server, Redis, and MongoDB).

EHR is hosting several legacy file- and API-based integrations with insurance providers on-premises. These systems are scheduled to be replaced over the next several years. There is no plan to upgrade or move these systems at the current time.

Users are managed via Microsoft Active Directory. Monitoring is currently being done via various open source tools. Alerts are sent via email and are often ignored.

#### Business requirements

+ On-board new insurance providers as quickly as possible 
+ Provide a minimum 99.9% availability for all customer-facing systems
+ Provide centralized visibility and proactive action on system performance and usage
+ Increase ability to provide insights into healthcare trends
+ Reduce latency to all customers
+ Maintain regulatory compliance
+ Decrease infrastructure administration costs
+ Make predictions and generate reports on industry trends based on provider data

#### Technical requirements

+ Maintain legacy interfaces to insurance providers with connectivity to both on-premises systems and cloud providers
+ Provide a consistent way to manage customer-facing applications that are container-based
+ Provide a secure and high-performance connection between on-premises systems and Google Cloud
+ Provide consistent logging, log retention, monitoring, and alerting capabilities
+ Maintain and manage multiple container-based environments
+ Dynamically scale and provision new environments
+ Create interfaces to ingest and process data from new providers

#### Executive statement

Our on-premises strategy has worked for years but has required a major investment of time and money in training our team on distinctly different systems, managing similar but separate environments, and responding to outages. Many of these outages have been a result of misconfigured systems, inadequate capacity to manage spikes in traffic, and inconsistent monitoring practices. We want to use Google Cloud to leverage a scalable, resilient platform that can span multiple environments seamlessly and provide a consistent and stable user experience that positions for future growth.

#### 01 

Anonymous users from all over the world access a public health information website hosted in an on-premises EHR data center. The servers that host this website are older, and users are complaining about sluggish response times. There has also been a recent increase of distributed denial-of-service attacks toward the website. The attacks always come from the same IP address ranges. EHR management has identified the public health information website as an easy, low risk application to migrate to Google Cloud. You need to improve access latency and provide a security solution that will prevent the denial-of-service traffic from entering your Virtual Private Cloud (VPC) network. What should you do?

> ☀︎ Deploy an external **HTTP(S) load balancer**, configure **Google Cloud Armor**, and move the application onto Compute Engine virtual machines ☀︎
>
> External HTTP(S) load balancer --- 可以改善存取延遲
>
> Cloud Armor --- 可以封鎖 DDoS 攻擊(Distributed Denial-of-Service)
>
> Firewall rules --- 無法封鎖惡意流量(malicious traffic)進 VPC，只在 VM level 封鎖
>
> GKE service --- does not expose a set of pods outside of a cluster
>
> GKE network policy --- only filters traffic between pods and services
>
> GKE internal load balancer --- will not load balance external traffic
>
> IAP (identity-aware proxy) --- not a fit, as anonymous users need access to the website

#### 02

EHR wants to connect one of their data centers to Google Cloud. The data center is in a remote location over 100 kilometers from a Google-owned point of presence. They can't afford new hardware, but their existing firewall can accommodate future throughput growth. They also shared these data points:

+ Servers in their on-premises data center need to talk to Google Kubernetes Engine (GKE) resources in the clould
+ Both on-premises servers and cloud resources are configured with private RFC 1918 addresses
+ The service provider has informed the customer that basic Internet connectivity is a best-effort service with no SLA

You need to recommend a connectivity option. What should you recommend?

> ☀︎ Provision a **Partner Interconnect connection** ☀︎
>
> Partner interconnect connection --- allows the customer to lower latency by connecting directly to a partner network that is directly connected to Google. This option will also allow the customer to use the lower bandwidth interfaces that they have on their current firewall.
>
> Carrier peering --- does not give private IP addressing across the connection
>
> A new Internet connection --- is not correct, because an additional Internet connection won't provide RFC1918 communications by itself
>
> Dedicated Interconnect connection --- would require the customer to buy new hardware to get a 10 gig interface for their firewall

#### 03

One of EHR's healthcare customers is an internationally renowned research and hospital facility. Many of their patients are well-known public personalities. Sources both inside and outside have tried many times to obtain health information on these patients for malicious purposes.  The hospital requires that patient information stored in Cloud Storage buckets not leave the geographic areas in which the buckets are hosted. You need to ensure that information stored in Cloud Storage buckets in the "Europe-west2" region does not leave that area. What should you do?

> ☀︎ **Enable Virtual Private Cloud Service Controls**, and create a **service perimeter** around the Cloud Storage resources ☀︎
>
> VPC Service Controls --- improves your ability to mitigate (減輕, 使緩和) the risk of data exfiltration (資料外洩) from Google Cloud services.
>
> Encrypt the data in the application on-premises before the data is stored in the `europe-west2` region --- Not correct, as encryptng the data does not stop data exfiltration 
> 只是加密資料並不能阻止資料外洩
>
> Assign the identity and access management (IAM) "storage.objectViewer" role only to users and service accounts that need to use the data --- Not correct, as IAM roles deal with identity-based access control, not context-aware perimeter security.
> IAM role 用來處理 **基於身分別的** 存取權控制，而非 **情境感知** 周圍資安
>
> Create an access control list (ACL) that limits access to the bucket to authorized users only, and apply it to the buckets in the "europe-west2" region --- Not correct, as Cloud Storage ACLs are a mechanism you can use to define who has access to your buckets and objects, as well as their level of access. ACLs do not stop data exfiltration.
> ACL用來定義哪些人可存取你的buckets＆objects、以及他們的存取等級，並不能拿來阻止資料外洩

#### 04

The EHR sales employees are a remote-based workforce that travels to different locations to do their jobs. Regardless of their location, the sales employees need to access web-based sales tools located in the EHR data center. EHR is retiring their current Virtual Private Network (VPN) infrastructure, and you need to move the web-based sales tools to a BeyondCorp access model. Each sales employee has a Google Workspace account and uses that account for single sign-on (SSO). What should you do?

> ☀︎ Create an **Identity-Aware Proxy** (IAP) connector that points to the sales tool application ☀︎
>
> Identity-Aware Proxy --- allows you to manage access to HTTP-based apps outside of Google Cloud
> 身份感知代理 可用來管理GoogleCloud以外，HTTP-based應用程式的存取權限
>
> Create a Google group for the sales tool application, and upgrade that group to a security group --- Not correct, as Google groups by themselves do not grant access to an application nor do they move an application to a beyond corp model.
> Google Group 不授予某一應用程式的存取權，也不負責移動應用程式
>
> Deploy an external HTTP(S) load balancer and create a custom Cloud Armor policy for the sales tool application --- Not correct, as Cloud Armor does not authenticate or authorize application access.
> Cloud Armor 不會驗證也不授權應用程式的可存取性
>
> > Authenticate (驗證): 確認是否真的為所宣稱的那個人)
> >
> > Authorize (授權): 根據使用者角色授予應有權限
>
> For every sales employee who needs access to the sales tool application, give their Google Workspace user account the predefined AppEngine Viewer role --- Not correct, as the application is installed in the datacenter, not in the AppEngine environment.
> 應用程式安裝在資料中心，不是App Engine環境

#### 05

You are responsible for ensuring that EHR's use of Google Cloud will pass an upcoming privacy compliance audit. What should you do? (Choose two.)

> ✅ Verify EHR's product usage against the list of compliant products on the *Google Cloud compliance page*
>
> > [Google Cloud compliance page](https://cloud.google.com/security/compliance/offerings?skip_cache=true#/regions=USA&industries=Healthcare_and_life_sciences&focusArea=Privacy) will give list of products which are HIPAA compliant
>
> ✅ Advise EHR to execute a Business Associate Agreement (BAA) with Google Cloud
>
> > BAA means HIPAA Business Associate amendment or Business Associate Agreement entered into between Google and Customer. 
> > With EHR being a leading provider of health record, software, this agreement is required.
>
> ❌ Use Firebase Authentication for EHR's user facing applications
>
> ❌ Implement Prometheus to detect and prevent security breaches on EHR's web-based applications
>
> ❌ Use GKE private clusters for all Kubernetes workloads

#### 06

You need to define the technical architecture for securely deploying workloads to Google Cloud. You also need to ensure that only verified containers are deployed using Google Cloud services. What should you do? (Choose two)

> ✅ Enable Binary Authorization on GKE, and sign containers as part of a CI/CD pipeline
>
> > [Binary Authorization](https://cloud.google.com/binary-authorization/docs/overview) : To ensure only verified containers are deployed
>
> ✅ Configure Container Registry to use vulnerability scanning to confirm that there are no vulnerabilities before deploying the workload
>
> > Automatically scan images for vulnerabilities with container analysis : To ensure deployment are secure and consistent
>
> ❌ Configure Jenkins to utilize Kritis to cryptographically sign a container as part of a CI/CD pipeline
>
> ❌ Configure Container Registry to only allow trusted service accounts to create and deploy containers from the registry

#### 07

You need to upgrade the EHR connection to comply with their requirements. The new connection design must support business-critical needs and meet the same network and security requirements. What should you do?

>✅ Add a new Dedicated Interconnect connection
>
>> You cannot change the link type on an Interconnect connection circuit from 10 Gbps to 100 Gbps.
>> 如果要遷移到 100Gbps 的話，需要先提供一條新的 100-Gbps Interconnect connection，再從 10-Gbps connection 遷移到 100-Gbps connection
>
>❌ Upgrade the bandwidth on the Dedicated Interconnect connection to 100 G
>
>❌ Add three new Cloud VPN connections
>
>❌ Add a new Carrier Peering connection

#### 08

You need to define the technical architecture for hybrid connectivity between EHR's on-premises systems and Google Cloud. You want to follow Google's recommended practices for production-level application. Considering the EHR Healthcare business and technical requirements, what should you do?

> ✅ Configure two *Dedicated Interconnect* connections in one metro (City) and two connections in another metro, and make sure the Interconnect connections are placed in different metro zones.
>
> > Requirements:
> >
> > + Provide a min 99.9% availability for all customer-facing systems
> > + Provide a secure and high-performance connection between on-premises systems and Google Cloud
> >
> > Above option builds us a 99.99% SLA dedicated interconnect, covering all business requirements
>
> ❌ Configure two *Partner Interconnect* connections in one metro (City), and make sure the Interconnect connections are placed in different metro zones
>
> > Above option (partner interconnect) builds us a 99.9% SLA partner interconnect, covering all business requirement.
> > But as the question stated "You want to follow Google's recommended practices for production-level applications", the **99.99%** SLA interconnect(dedicated or partner) is recommended for production-level application. ([Reference here](https://cloud.google.com/network-connectivity/docs/interconnect/tutorials/production-level-overview))
>
> ❌ Configure two *VPN connections* from on-premises to Google Cloud, and make sure the VPN devices on-premises are in separate racks
>
> > VPN is not suitable for the business requirements
>
> ❌ Configure *Direct Peering* between EHR Healthcare and Google Cloud, and make sure you are peering at least two Google locations
>
> > Direct peering is used for workspace, instead of DMZ, again - not suitable

#### 09

You are a developer on the EHR customer portal team. Your team recently migrated the customer portal application to Google Cloud. The load has increased on the application servers, and now the application is logging many timeout errors. You recently incorporated Pub/Sub into the application architecture, and the application is not logging any Pub/Sub publishing errors. You want to improve publishing latency. 

What should you do?

> ✅ Turn off Pub/Sub message batching
>
> > Cost of [Batching](https://cloud.google.com/pubsub/docs/publisher?hl=en#batching) is latency for individual messages. To minimize latency, batching should be turned off.
>
> ❌ Increase the Pub/Sub Total Timeout retry value
>
> ❌ Move from a Pub/Sub subscriber pull model to a push model
>
> ❌ Create a backup Pub/Sub message queue

#### 10

In the past, configuration errors put public IP addresses on backend servers that should not have been accessible from the Internet. You need to ensure that no one can put external IP addresses on backend Compute Engine instances and that external IP addresses can only be configured on frontend Compute Engine instances. What should you do?

> ✅ Create an **Organizational Policy** with a constraint to allow external IP addresses only on the frontend Compute Engine instances
>
> > Using an Organization Policy, you can restrict external IP addresses to specific VMs with constraints to control use of external IP addresses for your VM instances within an organization or a project
> >
> > [Limiting Public IPs on Google Cloud](https://cloud.google.com/blog/topics/developers-practitioners/limiting-public-ips-google-cloud)
>
> ❌ Revoke the comput.networkAdmin role from all users in the project with front end instances
>
> ❌ Create an Identity and Access Management (IAM) policy that maps the IT staff to the compute.networkAdmin role for the organization
>
> ❌ Create a custom Identity and Access Management (IAM) role named GCE_FRONTEND with the compute.addresses.create permission

#### 11

You are responsible for designing the Google Cloud network architecture for Google Kubernetes Engine. You want to follow Google best practices. Considering the EHR Healthcare business and technical requirements, what should you do to reduce the attack surface?

> ✅ Use a [private cluster](https://cloud.google.com/kubernetes-engine/docs/concepts/private-cluster-concept#overview) with a private endpoint with master authorized networks configured
>
> ❌ Use a public cluster with firewall rules and Virtual Private Cloud (VPC) routes
>
> ❌ Use a private cluster with a public endpoint with master authorized networks configured
>
> ❌ Use a public cluster with master authorized networks enabled and firewall rules

✅ ❌

___

## Mountkirk Game (Old)

#### Company Overview

Mountkirk Games makes online, session-based, multiplayer games for the most popular mobile platforms. They build all of their games using some server-side integration. Historically, they have used cloud providers to lease physical servers.

Due to the unexpected popularity of some of their games, they have had problems scaling their global audience, application servers MySQL databases, and analytics tools.

Their current model is to write game statics to files and send them through an ETL tool that loads them into a centralized MySQL database for reporting.

#### Solution Concept

Mountkirk Games is building a new game, which they expect to be very popular. They plan to deploy the game's backend on Google Compute Engine so they can capture streaming metrics run intensive analytics, and take advantage of its autoscaling server environment and integrate with a managed NoSQL database.

#### Business Requirements

+ Increase to a global footprint
+ Improve uptime `"downtime is loss of players
+ Increase efficiency of the cloud resources we use
+ Reduce latency to all customers

#### Technical Requirements

+ Requirements for Game Backend Platform
  1. Dynamically scale up or down based on game activity
  2. Connect to a managed NoSQL database service
  3. Run customize Linux distro
+ Requirements for Game Analytics Platform
  1. Dynamically scale up or down based on game activity 根據遊戲活動，動態規模化
  2. Process incoming data on the fly directly from the game servers
  3. Process data that arrives late because of slow mobile networks 
  4. Allow SQL queries to access at least 10 TB of historical data 可存取至少10TB歷史資料的SQL查詢
  5. Process files that are reqularly uploaded by users' mobile devices 處理使用者裝置定期上傳的檔案
  6. Use only fully managed services 

#### CEO Statement

Our last successful game did not scale well with our previous cloud provider, resulting in lower user adoption and affecting the game's reputation. Our investors want more key performance indicators (KPIs) to evaluate the speed and stability of the game, as well as other metrics that provide deeper insight into usage patterns so we can adapt the game to target users.

#### CTO Statement

Our current technology stack cannot provide the scale we need, so we want to replace MySQL and move to an environment that provides autoscaling, low latency load balancing, and frees us up from managing physical servers.

#### CFO Statement

We are not capturing enough user demographic data, usage metrics, and other KPIs. As a result, we do not engage the right users, we are not confident that our marketing is targeting the right users, and we are not selling enough premium Blast-Ups inside the games, which dramatically inpacts our revenue.

#### 01

Mountkirk Games wants you to design their new testing strategy. How should the test coverage differ from their existing backends on the other platforms?

> ✅ Tests should scale well beyond the prior approaches
>
> > Scaling is high priority SLI which CEO and CTO both explicitly mention:
> >
> > + CEO: 'Our last successful game did not scale well with our previous cloud provider'
> > + CTO: 'Our current technology stack cannot provide the scale we need'
>
> ❌ Test should include directly testing the Google Cloud Platform (GCP) infrastructure
>
> ❌ Unit tests are no longer required, only end-to-end tests
>
> ❌ Tests should be applied after the release is in the production environment

#### 02

Mountkirk Games has deployed their new backend on Google Cloud Platform (GCP). You want to create a through process for new versions of the backend before they are released to the public. You want the testing environment to scale in an economical way. How should you design the process?

> ✅ Create a scalable environment in GCP for simulating production load
>
> > From scenario: Requirements for Game Backend Platform
> >
> > 1. Dynamically scale up or down based on game activity
> > 2. Connect to a managed NoSQL database service
> > 3. Run customize Linux distro
> >
> > **Simulating production load in GCP can scale in an economical way.**
>
> ❌ Use the existing infrastructure to test the GCP-based backend at scale
>      One of the pain points about the existing infrastructure was precisely that the environment did not scale well.
>
> ❌ Build stress tests into each component of your application using resources internal to GCP to simulate load
>      It is a best practice to have a clear separation between test and production environments, and generating test load should not be done from a production environment.
>
> ❌ Create a set of static environments in GCP to test different levels of load, for example high/medium/low
>
> ​     Mountkirk Games wants the testing environment to scale as needed. Defining several static environments for specific levels of load goes against this requirement.

#### 03

Mountkirk Games wants to set up a continuous delivery pipeline. Their architecture includes many small services that they want to be able to update and roll back quickly. Mountkirk Games has the following requirements:

✑ Services are deployed redundantly across multiple regions in the US and Europe

✑ Only frontend services are exposed on the public internet

✑ They can provide a single frontend IP for their fleet of services

✑ Deployment artifacts are immutable

Which set of products should they use?

> ✅ Google Kubernetes Registry, Google Container Engine, Google HTTP(S) Load Balancer
>
> > Should be Google Kubernetes Engine, Google Container Registry, Google HTTP(S) Load Balancer
> >
> > + GKE: 
> >   Ideal for deploying small services that can be updated and rolled back quickly. 
> >   It's a best practice to manage services using immutable containers.
> > + Container Registry: 
> >   A single place for a team to manage Docker images for the services
> > + Cloud Load Balancing: 
> >   Support globally distributed services across multiple regions. 
> >   It provides a single global IP address that can be used in DNS records. 
> >   Using URL Maps, the requests can be routed to only the services that Mountkirk wants to expose.
>
> ❌ Google Cloud Storage, Google Cloud Dataflow, Google Compute Engine
>
> > It's NOT a continuous data processing pipeline that Mountkirk Games want to set up, but a delivery pipeline. Cloud Dataflow is a fully managed service for creating data processing pipeline
>
> ❌ Google Cloud Storage, Google App Engine, Google Network Load Balancer
>
> > Cloud Load Balancer distributes traffic to Compute Engine instances. App Engine and Cloud Load Balancer are parts of different solutions.
>
> ❌ Google Cloud Functions, Google Cloud Pub/Sub, Google Cloud Deployment Manager
>
> > You cannot reserve a single frontend IP for Cloud Functions. When deployed, an HTTP-triggered cloud function creates an endpoint with an automatically assigned IP.

#### 04

Mountkirk Games' gaming servers are not automatically scaling properly. Last month, they rolled out a new feature, which suddenly became very popular. A record number of users are trying to use the service, but many of them are getting 503 errors and very slow response times. What should they investigate first?

> ✅ Verify that the project quota hasn't been exceeded
>
> > 503 is service unavailable error. If the database was online everyone would get the 503 error (server is down).
>
> ❌ Verify that the database is online
>
> ❌ Verify that the new feature code did not introduce any performance bugs
>
> ❌ Verify that the load-testing team is not running their tool against production

#### 05

Mountkirk Games needs to create a repeatable and configurable mechanism for deploying isolated application. Developers and tester can access each other's environments and resources, but they cannot access staging or production resources. The staging environment needs access to some services from production.

What should you do to isolate development environments from staging and production?

> ✅ Create one project for development, a second for staging and a third for production
>
> ❌ Create a project for development and test and another for staging and production
>
> ❌ Create a network for development and test and another for staging and production
>
> ❌ Create one subnetwork for development and another for staging and production

#### 06

Mountkirk Games wants to set up a real-time analytics platform for their new game. The new platform must meet their technical requirements. Which combination of Google technologies will meet all of their requirements?

> ✅ Cloud Dataflow, Cloud Storage, Cloud Pub/Sub, and BigQuery
>
> > Ingest millions of *streaming events* per second from anywhere in the world with *Cloud Pub/Sub*, powered by Google's unique, high-speed private network. 
> >
> > *Process the streams with Cloud Dataflow* to ensure reliable, exactly-once, low-latency data transformation. 
> >
> > Stream the transformed data into *BigQuery*, the cloud-native data warehousing service, for *immediate analysis via SQL or popular visualization tools.*
> >
> > From scenario: They plan to deploy the game's backend on Google Compute Engine so they can capture streaming metrics, run intensive analytics
> >
> > Requirements for Game Analytics Platform:
> >
> > 1. Dynamically scale up or down based on game activity
> > 2. Process incoming data on the fly directly from the game servers
> > 3. Process data that arrives late because of slow mobile networks
> > 4. Allow SQL queries to access at least 10 TB of historical data
> > 5. Process files that are regularly uploaded by user's mobile devices
> > 6. Use only fully managed services
>
> ❌ Kubernetes Engine, Cloud Pub/Sub, and Cloud SQL
>
> ❌ Cloud SQL, Cloud Storage, Cloud Pub/Sub, and Cloud Dataflow
>
> ❌ Cloud Dataproc, Cloud Pub/Sub, Cloud SQL, and Cloud Dataflow
>
> ❌ Cloud Pub/Sub, Compute Engine, Cloud Storage, and Cloud Dataproc

---

## Mountkirk Game (old_ver.2)

#### Company Overview 

Mountkirk Games makes online, session-based, multiplayer games for mobile platforms. They build all of their games using some server-side integration.

Historically, they have used cloud providers to lease physical servers.

Due to the unexpected popularity of some of their games, they have had problems scaling their global audience, application servers, MySQL databases, and analytics tools.

Their current model is to write game statistics to files and send them through an ETL tool that loads them into a centralized MySQL database for reporting.

#### Solution Concept -

Mountkirk Games is building a new game, which they expect to be very popular. They plan to deploy the games' backend on Google Compute Engine so they can capture streaming metrics, run intensive analytics, and take advantage of its autoscaling server environment and integrate with a managed NoSQL database.

#### Business Reuirements - 

Increase to a global footprint

Improve uptime `"downtime is loss of players

Increase efficiency of the cloud resources we use

Reduce latency to all customers

#### Technical Requirements -

+ Requirements for Game Backend Platform
  + Dynamically scale up or down based on game activity 
  + ⛄️Connect to a transactional database service to manage user profiles and game state
  + ⛄️ Store game activity in a timeseries database service for future analysis
  + ⛄️ As the system scales, ensure that data is not lost due to processing backlogs
  + Run hardened Linux distro
+ Requirements for Game Analytics Platform
  + Dynamically scale up or down based on game activity 
  + Process incoming data on the fly directly from the game servers 迅速處理數據
  + Process data that arrives late because of slow mobile servers
  + Allow queries to access at least 10 TB of historical data
  + Process files that are regularly uploaded by users' mobile devices

#### Executive Statement -

Our last successful game did not scale well with out previous cloud provider, resulting in lower user adoption and affecting the game's reputation. Our investors want more key performance indicators (KPIs) to evaluate the speed and stability of the game, as well as other metrics that provide deeper insight into usage patterns so we can adapt the game to target users. Additionally, our current technology stack cannot provide the scale we need, so we want to replace MySQL and move to an environment that provides autoscaling, low latency load balancing, and frees us up from managing physical servers.

#### 01

Mountkirk Games wants to migrate from their current analytics and statics reporting model to one that meets their technical requirements on Google Cloud Platform.

Which two steps should be part of their migration plan? (Choose two)

> ✅ Evaluate the impact of migrating their current batch ETL code to Cloud Dataflow
>
> > Must have --- ETL is definitely what Cloud Dataflow does. 
> > Cloud Dataflow --- Stream mobile devices data (Fully managed service)
>
> ✅ Write a schema migration plan to denormalize data for better performance in BigQuery
>
> > BigQUery --- Intensive Analytics + historic data (Fully managed service)
>
> ❌ Draw an architecture diagram that shows how to move from a single MySQL database to a MySQL cluster
>
> > Cloud SQL is NOT the best for hug volumes of data, plus not real time data
>
> ❌ Load 10 TB of analytics data from a previous game into a Cloud SQL instance, and run test queries against the full dataset to confirm that they complete successfully
>
> ❌ Integrate Cloud Armor to defend against possible SQL injection attacks in analytics files uploaded to Cloud Storage

#### 02

You need to analyze and define the technical architecture for the compute workloads for your company, Mountkirk Games. Considering the Mountkirk Games business and technical requirements, what should you do?

> ✅ Create a global load balancer with managed instance groups and autoscaling policies. Use non-preemptible Compute Engine instances
>
> > Termination of preemptive VM might affect gaming experience, hence not a good choice.
>
> ❌ Create network load balancers. Use preemptible Compute Engine instances
>
> ❌ Create network load balancers. Use non-preemptible Compute Engine instances
>
> ❌ Create a global load balancer with managed instances groups and autoscaling policies. Use preemptible Compute Engine instances

#### 03

Mountkirk Games wants to design their solution for the future in order to take advantage of cloud and technology improvements as they become available. Which two steps should they take? (Choose two)

> ✅ Store as much analytics and game activity data as financially feasible today so it can be used to train machine learning models to predict user behavior in the future
>
> ✅ Begin packaging their game backend artifacts in container images and running them on Google Kubernetes Engine to improve the ability to scale up or down based on game activity
>
> ❌ Set up a CI/CD pipeline using Jenkins and Spinnaker to automate canary deployment and improve development velocity
>
> > Cloud Build is the Google Solution for CI/CD and requires Dockerfile, hence CI/CD pipe line code for Jenkins will not be useful in the future.
> >
> > Spinnaker - 開源持續交付軟體平台，由Netflix開發，可在K8S, GCP, AWS, Azure, Oracle Cloud實作
> >
> > Canary deployment - 只有一套系統，逐漸替換這套系統。跟礦坑的金絲雀一樣，此模式可以提早預警可能出現的錯誤。
> >
> > Development velocity - 開發速度
>
> ❌ Adopt a schema versioning tool to reduce downtime when adding new game features that require storing additional player data in the database
>
> > Schema versioning tool is not related for future google services.
>
> ❌ Implement a weekly rolling maintenance process for the Linux virtual machines so they can apply critical kernel patches and package updates and redue the risk of 0-day vulnerabilities
>
> > Google does the patching on all VMs, so it's not relevant.

#### 04

Mountkirk Games wants you to design a way to test the analytics platform's resilience to changes in mobile network latency. What should you do?

> ✅ Deploy failure injection software to the game analytics platform that can inject additional latency to mobile client analytics traffic
>
> > Add latency to mobile network --- to test the analytics platform's resilience to changes in mobile network latency. 
>
> ❌ Build a test client that can be run from a mobile phone emulator on a Compute Engine virtual machine, and run multiple copies in Google Cloud Platform regions all over the world to generate realistic traffic
>
> ❌ Add the ability to introduce a random amount of delay before beginning to process analytics files uploaded from mobile devices
>
> > Adds delay at beginning of file processing, does not add delay/latency in mobile network
>
> ❌ Create an opt-in beta of the game that runs on players' mobile devices and collects response times from analytics endpoints running in Google Cloud Platform regions all over the world

#### 05

You need to analyze and define the technical architecture for the database workloads for your company, Mountkirk Games. Considering the business and technical requirements, what should you do?

> ✅ Use Cloud Bigtable for <u>time series data</u>, use Cloud Spanner for <u>transactional data</u>, and user BigQuery for <u>historical data queries</u>
>
> > Storing time-series data in Cloud Bigtable is a natural fit.
> >
> > Cloud Spanner scales horizontally and serves data with low latency, while maintaining transactional consistency and industry-leading 99.999% (five 9s) availability - 10x less downtime than four nines (<5 minutes per year).
> > Cloud Spanner helps future-proof your database backend. After you load your data into BigQuery, you can query the data in your tables.
> >
> > BigQuery supports two types of queries: Interactive queries, Batch queries
>
> ❌ Use Cloud SQL for time series data, and use Cloud Bigtable for historical data queries
>
> ❌ Use Cloud SQL to replace MySQL, and use Cloud Spanner for historical data queries
>
> ❌ Use Cloud Bigtable to replace MySQL, and use BigQuery for historical data queries

#### 06

Which managed storage option meets Mountkirk's technical requirement for storing game activity in a time series database service?

> ✅ Cloud Bigtable
>
> > Google says we can store time series vehicle data in Bigtable which can later be used for analytical processing using BigQuery
>
> ❌ Cloud Spanner
>
> ❌ BigQuery
>
> > BigQuery can be used to analyse the data, but not a suitable target for time series.
>
> ❌ Cloud Datastore

#### 07

You are in charge of the new Game Backend Platform architecture. The game communicates with the backend over a REST API. You want to follow Google-recommended practices. How should you design the backend?

> ✅ Create an instance template for the backend. For every region, deploy it on a multi-zone managed instance group. Use an L7 load balancer
>
> > L4 LB: Offers traffic management of transactons at the network protocol layer (TCP/UDP)
> >
> > L7 LB: Works at the highest level of the OSI model and bases its routing decisions on various characteristics of the HTTP/HTTPS header. (Mountrik requirement is global, so L7)
>
> ❌ Create an instance template for the backend. For every region, deploy it on a multi-zone managed instance group. Use an L4 load balancer
>
> ❌ Create an instance template for the backend. For every region, deploy it on a single-zone managed instance group. Use an L4 load balancer
>
> ❌ Create an instance template for the backend. For every region, deploy it on a single-zone managed instance group. Use an L7 load balancer

---

## Mountkirk Game (New)

#### Company overview

Mountkirk Games makes online, session-based, multiplayer games for mobile platforms. They have recently started expanding to other platforms after successfully migrating their on-premises environments to Google Cloud.

Their most recent endeavor is to create a retro-style first-person shooter (FPS) game that allows hundreds of simultaneous players to join a geo-specific digital arena from multiple platforms and locations. A real-time digital banner will display a global leaderboard of all the top players across every active arena.

#### Solution concept

Mountkirk Games is building a new multiplayer game that they expect to be very popular. They plan to deploy the game's backend on Google Kubernetes Engine so they can scale rapidly and use Google's global load balancer to route players to the closest regional game arenas. In order to keep the global leader board in sync, they plan to use a multi-region Spanner cluster.

#### Existing technical environment

The existing environment was recently migrated to Google Cloud, and five games came across using lift-and-shift virtual machine migrations, with a few minor exceptions.

Each new game exists in an isolated Google Cloud project nested below a folder that maintains most of the permissions and network policies. Legacy games with low traffic have been consolidated into a single project. There are also separate environments for development and testing.

#### Business requirements

+ Support multiple gaming platforms 支援多遊戲平台
+ Support multiple regions 支援多區域
+ Support rapid iteration of game features 支援遊戲功能快速迭代
+ Minimize latency 最小化延遲
+ Optimize for dynamic scaling 動態可擴充性優化
+ Use managed services and pooled resources 管理化服務 ＆資源池(?)
+ Minimize costs 最小化成本

#### Technical requirements

+ Dynamically scale based on game activity
+ Publish scoring data on a near real-time global leaderboard
+ Store game activity logs in structured files for future analysis
+ Use GPU processing to render graphics server-side for multi-platform support
+ Supports eventual migration of legacy games to this new platform

#### Executive statement

Our last game was the first time we used Google Cloud, and it was a tremendous success. We were able to analyze player behavior and game telemetry in ways that we never could before. This success allowed us to bet on a full migration to the cloud and to start building all-new games using cloud-native design principles. Our new game is our most ambitious to date and will open up doors for us to support more gaming platforms beyond mobile. Latency is our top priority, although cost management is the next most important challenge. As with our first cloud-based game, we have grown to expect the cloud to enable advanced analytics capabilities so we can rapidly iterate on our deployments of bug fixes and new functionality.

#### 01

You are the data compliance officer for Mountkirk Games and must protect customers' personally identifiable information (PII). Mountkirk Games wants to make sure they can generate anonymized usage reports about their new game and delete PII data after a specific period of time. The solution should have minimal cost. You need to ensure compliance while meeting business and technical requirements. What should you do?

> ✅ Archive audit logs in BigQuery, and generate reports using Google Data Studio
>
> BigQuery --- allows easy querying for report generation, with low storage costs
> BigQuery查詢/生成報表容易，儲存成本低
>
> ❌ Archive audit logs in Cloud Storage, and manually generate reports --- Not correct, as Cloud Storage is an object store with no query language access for report generation.
> Cloud Storage不支持SQL查詢，生成不了報表
>
> ❌ Write a Cloud Logging filter to export specific date ranges to Pub/Sub --- Not correct, as it does not address log storage for data retention
> 沒有提到 log 資料留存的空間
>
> ❌ Archive user logs on a locally attached persistent disk, and cat them to a text file for auditing --- Not correct, as long term storage in persistent disk is expensive
> 把資料長期存在*持久性區塊儲存空間*很貴

#### 02

Ｍountkirk Games wants you to make sure their new gaming platform is being operated according to Google best practices. You want to verify that Google recommended security best practices are being met while also providing the operations teams with the metrics they need. What should you do? (Choose two)

> ✅ Ensure that you aren't running privileged containers
> + Correct, as this is High Priority according to Google best practices
>
> ✅ Ensure that you are using the native logging mechanisms
> + Correct, as this is High Priority according to Google best practices
>
> ❌ Ensure that you are using obfuscated Tags (混淆的tag) on workloads
> + Not correct, as tags should be readable and useful to the operations teams when they are working on the clusters
>
> ❌ Ensure that workloads are not using securityContext to run as a group
> + Not correct, as this may be required for some workloads
>
> ❌ Ensure that each cluster is running GKE metering so each team can be charged for their usage
> + Not correct, bcuz although from a business process this may be useful, it won't impact the operations or security of the cluster 不會影響叢集的安全性/操作性

#### 03

You need to implement Virtual Private Cloud (VPC) Service Controls for Mountkirk Games. Mountkirk Games wants to allow Cloud Shell usage by its developers. Developers should not have full access to managed services. You need to balance these conflicting goals with Mountkirk Games' business requirements. What should you do?

> ✅ Create a **service perimeter around only the projects that handle sensitive data**, and do not grant your developers access to it 
>
> VPC Service Controls --- protects data
> Cloud Shell --- facilitates rapid iteration of Google Cloud architecture changes 
>
>  VPC Service Controls  --- do not directly affect scaling of Google Cloud architecture.
>
> A security perimeter --- is not a business requirement, but rapid iteration is.
>
> ❌ Include all developers in an access level associated with the service perimeter, and allow them to use Cloud Shell --- Not correct, as it works but does not scale, and we do not want to give users this much access

#### 04

Your new game running on Google Cloud is in public beta, and you want to design meaningful service level objectives (SLOs) before the game becomes generally available. What should you do?

> ✅ Define one SLO as 99% HTTP requests return the 2xx status code. Define the other SLO as 99% requests return within 100ms. 
>
> > Correct, as it clearly defines **the service level indicators and how to measure them**
>
> ❌ Define one SLO as 99.9% game server availability. Define the other SLO as less than 100-ms latency --- Incorrect, as it doesn't clearly define how to measure both the availability and latency
>
> ❌ Define one SLO as service availability that is the same as Google Cloud's availability. Define the other SLO as 100-ms latency --- Incorrect, as Google Cloud availability has an impact on customer availability, but it is only one factor. Also, for different Google Cloud products, the availability could be different.
>
> ❌ Define one SLO as total uptime of the game server within a week. Define the other SLO as the mean response time of all HTTP requests that are less than 100 ms --- Incorrect, as there is not objective for the server uptime. 

#### 05

HRL wants you to help them bring existing recorded video content to new fans in emerging regions. Considering the HRL business and technical requirements, what should you do?

> ✅ Use Cloud CDN to cache the video content from HRL's existing public cloud provider.
>
> > Cloud CDN --- can be used to cache data hosted on other cloud providers and supports large objects such as video
> > 可用來快取存放在其它雲服務商的資料，且支援像是影片的大型物件
>
> ❌ Serve the video content directly from a multi-region Cloud Storage bucket --- Incorrect, as a multi-region bucket does not serve all global areas with similar latency.
>
> ❌ Use Apigee Edge to cache the video content from HRL's existing public cloud provider --- Incorrect, as Apigee Edge is not designed to cache data larger than 512 KB.
>
> ❌ Replicate the video content in Google Kubernetes Engine clusters in regions close to the fans --- Incorrect, as replicating the video content introduces unnecessary complexity.

#### 06

You need to optimize batch file transfers into Cloud Storage for Mountkirk Games' new Google Cloud solution. The batch files contain game statistics that need to be staged in Cloud Storage and be processed by an extract transform load (ETL) tool. What should you do?

> ✅ Use gsutil to batch copy the files in parallel
>
> > If you have a large number of files to transfer, you can perform a parallel multi-threaded/multi-processing copy using the top-level `gsutil -m` option
> >
> > `gsutil -m cp -r dir gs://my-bucket`
> >
> > [Copy Files and Objects](https://cloud.google.com/storage/docs/gsutil/commands/cp)
> >
> > [Optimizing your Cloud Storage performance_Google Cloud Performance Atlas](https://cloud.google.com/blog/products/gcp/optimizing-your-cloud-storage-performance-google-cloud-performance-atlas)
>
> ❌ Use gsutil to batch move files in sequence
>
> ❌ Use gsutil to extract the files as the first part of ETL
>
> ❌ Use gsutil to load the files as the last part of ETL

#### 07

You are implementing Firestore for Mountkirk Games. Mountkirk Games wants to give a new game programmatic access to a legacy game's Firestore database. Access should be as restricted as possible. What should you do?

> ✅ Create a service account (SA) in the legacy game's Google Cloud project, add this SA in the new game's IAM page, and then give it the Firebase Admin role in both projects
>
> > Firebase Admin role --- key point
> >
> > Cross-Projects Resource sharing via service account
>
> ❌ Create a service account (SA) in the legacy game's Google Cloud project, add a second SA in the new game's IAM page, and then give the Organization Admin role to both SAs
>
> ❌ Create a service account (SA) in the legacy game's Google Cloud project, give the SA the Organization Admin role, and then give it the Firebase Admin role in both projects
>
> ❌ Create a service account (SA) in the legacy game's Google Cloud project, give it the Firebase Admin role, and then migrate the new game to the legacy game's project
>
> > We should have separate projects for every environment for each application. We should NOT add new application to an existing project.

#### 08

Mountkirk Games wants to limit the physical location of resources to their operating Google Cloud regions. What should you do?

> ✅ Configure an organizational policy which constrains where resources can be deployed
>
> > You can limit the physical location of a new resource with the <u>Organization Policy Service resource locations constraint</u>. You can use the location property of a resource to identify where it is deployed and maintained by the service. 
> >
> > For data-containing resources of some Google Cloud services, this property also reflects the location where data is stored. This constraint allows you to define the allowed Google Cloud locations where the resources for supported services in your hierarchy can be created.
> >
> > After you define resource locations, this limitation will apply only to newly-created resources. Resources you created before setting the resource locations constraint will continue to exist and perform their function.
> >
> > [Restricting Resource Locations](https://cloud.google.com/resource-manager/docs/organization-policy/defining-locations)
>
> ❌ Configure IAM conditions to limit what resources can be configured
>
> ❌ Configure the quotas for resources in the regions not being used to 0
>
> ❌ Configure a custom alert in Cloud Monitoring so you can disable resources as they are created in other regions

#### 09

You need to implement a network ingress for a new game that meets the defined business and technical requirements. Mountkirk Games wants each regional game instance to be located in multiple Google Cloud regions. What should you do?

> ✅ Configure Ingress for Anthos with a global load balancer and Google Kubernetes Engine
>
> > Since it's a multiple regions game, need  multi-GKE or multi-MIG.
> >
> > To configure the ingress between multi-GKE, use kubemci or Anthos.
> >
> > kubemci --- was deprecated in favor of Anthos.
> >
> > Ingress for Anthos is the recommended way to deploy **multi-cluster ingress**
> >
> > [Concepts of Multi Cluster Ingress](https://cloud.google.com/kubernetes-engine/docs/concepts/multi-cluster-ingress)
> >
> > [How to: Multi Cluster Ingress](https://cloud.google.com/kubernetes-engine/docs/how-to/multi-cluster-ingress)
>
> ❌ Configure a global load balancer connected to a managed instance group running Compute Engine instances
>
> ❌ Configure kubemci with a global load balancer and Google Kubernetes Engine
>
> ❌ Configure a global load balancer with Google Kubernetes Engine

#### 10

You development teams release new versions of games running on Google Kubernetes Engine (GKE) daily. You want to create service level indicators (SLIs) to evaluate the quality of the new versions from the user's perspective. What should you do?

> ✅ Create Request Latency and Error Rate as service level indicators
>
> > **SLI** is the metrics for the level of service provided to end users. Real numbers of the performance. Example: Request latency to be less than 500ms in the last 15 minutes with a 95% percentile.
> >
> > **SLO** - Target level for the reliability of your service
> >
> > **SLA** - The agreement that you make with the end users
>
> ❌ Create CPU Utilization and Request Latency as service level indicators
>
> ❌ Create GKE CPU Utilization and Memory Utilization as service level indicators
>
> ❌ Create Server Uptime and Error Rate as service level indicators

#### 11

Mountkirk Games wants you to secure the connectivity from the new gaming application platform to Google Cloud. You want to streamline the process and follow Google-recommended practices. What should you do?

> ✅ Configure Workload Identity and service accounts to be used by the application platform
>
> > Workload Identity is the recommended way to access Google Cloud services from applications running within GKE due to its improved security properties and manageability.
> >
> > [Workload Identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity)
>
> ❌ Use Kubernetes Secrets, which are obfuscated by default. Configure these Secrets to be used by the application platform
>
> ❌ Configure Kubernetes Secrets to store the secret, enable Application-Layer Secrets Encryption, and use Cloud Key Management Service (Cloud KMS) to manage the encryption keys. Configure these Secrets to be used by the application platform
>
> ❌ Configure HashiCorp Vault on Compute Engine, and use customer managed encryption keys and Cloud Key Management Service (Cloud KMS) to manage the encryption keys. Configure these Secrets to be used by the application platform

#### 12

Your development team has created a mobile game app. You want to test the new mobile app on Android and iOS devices with a variety of configurations. You need to ensure that testing is efficient and cost-effective. What should you do?

> ✅ Upload your mobile app to the Firebase Test Lab, and test the mobile app on Android and iOS devices.
>
> > + Firebase Test Lab is a cloud-based app testing infrastructure that lets you test your app on a range of devices and configurations, so you can get a better idea of how it'll perform in the hands of live users.
> > + Firebase Test Lab Run tests on a wide range of Android and iOS devices hosted by Test Lab.
> >
> > [Firebase Test Lab](https://firebase.google.com/docs/test-lab)
>
> ❌ Create Android and iOS VMs on Google Cloud, install the mobile app on the VMs, and test the mobile app
>
> ❌ Cread Android and iOS containers on GKE, install the mobile app on the containers, and test the mobile app
>
> ❌ Upload your mobile app with different configurations to Firebase Hosting and test each configuration

---

## 🧩 TerramEarth (old) 

#### Company Overview -

TerramEarth manufactures heavy equipment for the mining and agricultural industries: about 80% of their business is from mining and 20% from agriculture. They currently have over 500 dealers and service centers in 100 countries. Their mission is to build products that make their customers more productive.

#### Company background -

TerramEarth was formed in 1946, when several small, family owned combined to retool after World War II. The company cares about their employees and customers and considers them to be extended members of their family.

TerramEarth is proud of their ability to innovate on their core products and find new markets as their customers' needs change. For the past 20 years, trends in the industry have been largely toward increasing productivity by using larger vehicles with a human operator.

#### Solution Concept -

There are 20 million TerramEarth vehicles in operation that collect 120 fields of data per second. Data is stored locally on the vehicle and can be accessed for analysis when a vehicle is serviced. The data is downloaded via a maintenance port. This same port can be used to adjust operational parameters, allowing the vehicles to be upgraded in the field with new computing modules.

Approximately 200,000 vehicles are connected to a cellular network, allowing TerramEarth to collect data directly. At a rate of 120 fields of data per second with 22 hours of operation per day, Terram Earth collects a total of about 9TB/day from these connectd vehicles.

#### Existing Technical Environment - 

```
[Bulldozer_M1]-\
                 \
[Bulldozer_M2] --- [FTP Servers] -->[ETL] --> [Data warehouse]
                 /
[Truck] --------/
```

TerramEarth's existing architecture is composed of Linux-based systems that reside in a data center. These systems gzip CSV files from the field and upload via FTP, transform and aggregate(聚集) them, and place the data in their data warehouse. Because this process takes time, aggregated reports are based on data that is 3 weeks old.

With this data, TerramEarth has been able to preemptively stock replacement parts and reduce unplanned downtime of their vehicles by 60%. However, because the data is stale, some customers are without their vehicles for up to 4 weeks while they wait for replacement parts.

#### Business Requirements -

Decrease unplanned vehicle downtime to less than 1 week, without increasing the cost of carrying surplus inventory 

Support the dealer network with more data on how their customers use their equipment to better position new products and services

Have the ability to partner with different companies "especially with seed and fertilizer suppliers in the fast-growing agricultural business" to create compelling joint offerings for their customers.

#### CEO Statement -

We have been successful in capitalizing on the trend toward larger vehicles to increase the productivity of our customers. Technological change is occurring rapidly, and TerramEarth has taken advantage of connected devices technology to provide our customers with better services, such as our intelligent farming equipment. With this technology, we have been able to increase farmers' yields by 25%, by using past trends to adjust how our vehicles operate. These advances have led to rapid growth of our agricultural product line, which we expect will generate 50% of our revenues by 2020.

#### CTO Statement -

Our competitive advantage has always been in the manufacturing process, with our abilty to build better vehicles for lower cost than our competitors. However, new products with different approaches are constantly being developed, and I'm concerned that we lack the skills to undergo the next wave of transformations in our industry. Unfortunately, our CEO doesn't take technology obsolescence(科技廢退) seriously and he considers the many new companies in our industry to be riche player. My goals are to build our skills while addressing immediate market needs through incremental innovations.

#### 01

TerramEarth's CTO wants to use the raw data from connected vehicles to help identify approximately when a vehicle in the field will have a catastrophic failure.

You want to allow analysts to centrally query the vehicle data. Which architecture should you recommend?

> ✅
>
> ```
>  [IoT]         [IoT]          [IoT]
>   \FTP          |FTP         /FTP
>    [Google Cloud Load Balancing]
>                 |           FTP
>      [Google Container Engine]
>                 |
>          [Cloud Pub/Sub]
>                 |
>         [Cloud Dataflow]
>                 |
>            [BigQuery]
>            /    |   \
>            [Analysts]
> ```
>
> > The push endpoint can be a load balancer; a container cluster can be used; use Cloud Pub/Sub for Stream Analytics.
> >
> > + Cloud Dataflow is essential to transform, enrich and store telemetry data by using distributed data pipelines
> > + Cloud Pub/Sub is essential to handle the streams of vehicle data while at the same time decoupling the specifics of the backend processing implementation
> >
> > Now choose between:
> >
> > 1. Cloud SQL vs BigQuery for analytics
> >    + BigQuery is no doubt the preferred choice for analytics
> >    + Cloud SQL does not scale to this sort of data volume
> >       (9TB/day + data coming through when vehicles are serviced)
> > 2. GKE (with or without Anthos) + Cloud Load balancing vs App Engine
> >    + GKE with Cloud Load Balancing is better than App Engine
> >      + You can have multiple GKE clusters in different regions.
> >      + Cloud LB can send requests to the cluster in the region that is closest to the vehicle.
> >      + Pros: minimize latency; make the feedback loop more real-time
> >    + App Engine is a **regional service**.
>
> ❌ [IoT] *3 ⎼(FTP)→[App Engine Flexible Environment]→
>            [Cloud Pub/Sub]→[Cloud Dataflow]→
>                [BigQuery]→[Analysts] *3
>
> ❌ [IoT] *3 ⎼(FTP)→[Google Cloud Load Balancing]⎼(FTP)→
>            [Google Container Engine]→[Cloud Pub/Sub]→
>                [Cloud Dataflow]→[Cloud SQL]→[Analysts] *3
>
> ❌ [IoT] *3 ⎼(FTP)→[App Engine Flexible Environment]→
>            [Cloud Pub/Sub]→[Cloud Dataflow]→
>                [Cloud SQL]→[Analysts] *3

#### 02

The TerramEarth development team wants to create an API to meet the company's business requirements. You want the development team to focus their development effort on business value versus creating a custom framework.

Which method should they use?

> ✅ Use Google App Engine with Google Cloud Endpoints. Focus on an API for dealers and partners
>
> > Develop, deploy, protect and monitor your APIs with Google Cloud Endpoints. Using an Open API Specification or one of our API frameworks, Cloud Endpoints gives you the tools you need for every phose of API development
> >
> > 使用 Endpoints Framework，就不用再部署一個第三方web伺服器（例如：Apache Tomcat、Gunicorn）
>
> ❌ Use Google App Engine with a JAX-RS Jersey Java-based framework. Focus on an API for the public
>
> ❌ Use Google App Engine with the Swagger (Open API Specification) framework. Focus on an API for the public
>
> ❌ Use Google Container Engine with a Django Python container. Focus on an API for the public
>
> ❌ Use Google Container Engine with a Tomcat container with the Swagger (Open API Specification) framework. Focus on an API for dealers and partners

#### 03

Your development team has created a structured API to retrieve vehicle data. They want to allow third parties to develop tools for dealerships that use this vehicle event data. You want to support delegated authorization against this data. What should you do?

> ✅ Build or leverage an OAuth-compatible access control system
>
> > Cloud Platform APIs support OAuth 2.0, and scopes provide granular authorization over the methods that are supported. Cloud Platform supports both service- account and user-account OAuth, also called three-legged OAuth.
> >
> > [Delegate App Authorization with OAuth2](https://cloud.google.com/architecture/framework#delegate_application_authorization_with_oauth2)
> >
> > [Identity and Authentication](https://cloud.google.com/blog/products/identity-security/identity-and-authentication-the-google-cloud-way)
>
> ❌ Build SAML 2.0 SSO compatibility into your authentication system
>
> > SAML is an authentication system 驗證; while OAuth is an authorization system 授權.
> >
> > 兩者都支援單一登入 (SSO, single sign-on). SAML 給使用者用; OAuth 給應用程式app用
>
> ❌ Restrict data access based on the source IP address of the partner systems
>
> ❌ Create secondary credentials for each dealer that can be given to the trusted third party

#### 04

TerramEarth plans to connect all 20 million vehicles in the field to the cloud. THis increases the volume to 20 million 600 byte records a second for 40 TB an hour. How should you design the data ingestion?

> ✅ Vehicles write data directly to Google Cloud Pub/Sub
>
> > Pub/Sub can handle more streaming data than BigQuery 
> >
> > + Pub/Sub: 120,000,000 kB/minute, 2 GB/per second in large regions
> > + BigQuery: 1Gb/per second
>
> ❌ Vehicles write data directly to GCS
>
> ❌ Vehicles stream data directly to Google BigQuery
>
> ❌ Vehicles continue to write data using the existing system (FTP)

#### 05

You analyzed TerramEarth's business requirement to reduce downtime, and found that they can achieve a majority of time saving by reducing customer's wait time for parts. You decided to focus on reduction of the 3 weeks aggregate reporting time.

Which modifications to the company's processes should you recommend?

> ✅ Increase fleet cellular connectivity to 80%, migrate from FTP to streaming transport, and develop machine learning analysis of metrics
>
> > The Avro binary format is the preferred format for loading compressed data. Avro data is faster to load because the data can be read in parallel, even when the data blocks are compressed.
> >
> > Cloud Storage supports streaming transfers with the gsutil tool or boto library, based on HTTP chunked transfer encoding. Streaming data lets you stream data to and from your Cloud Storage account as soon as it becomes available without requiring that the data be first saved to a separate file. Streaming transfers are useful if you have a process that generates data and you do not want to buffer it locally before uploading it, or if you want to send the result from a computational pipeline directly into Cloud Storage.
> >
> > [Uploads and downloads](https://cloud.google.com/storage/docs/streaming)
> >
> > [Introduction to loading data](https://cloud.google.com/bigquery/docs/loading-data)
> >
> > + Cellular connectivity: greatly improve the freshness of data
>
> ❌ Migrate from CSV to binary format, migrate from FTP to SFTP transport, and development machine learning analysis of metrics
>
> > Machine learning - a good means toward the end of reducing downtime
> >
> > Shuffling formats and transport doesn't directly help at all
>
> ❌ Migrate from FTP to streaming transport, migrate from CSV to binary format, and develop machine learning analysis of metrics
>
> > Moving to streaming can improve the freshness of the information
> >
> > Changing the format doesn't directly help at all
>
> ❌ Migrate from FTP to SFTP transport, develop machine learning analysis of metrics, and increase dealer local inventory by a fixed factor
>
> > Nothing helps except for machine learning analysis

06

Which of TerramEarth's legacy enterprise processes will experience significant change as a result of increased Google Cloud Platform adoption?

> ✅ Capacity planning, TCO calculations, opex/capex allocation
>
> > Management concerns:
> >
> > + Rapid provision of resources (infrastructure) for growing
> > + Cost management such as --
> >   Cost optimization in Infrastructure, 
> >   trade up from capital expenditures(Capex) for ongoing operating expenditures (Opex)
> > + Total cost of ownership (TCO)
> >
> > ---
> >
> > Capacity planning --- for cloud is different from those for on-premises data centers
> >
> > TCO calculations --- adjusted, as TerramEarth is using services, not leasing/buying servers
> >
> > OpEx/CapEx allocation --- adjusted, as services are consumed vs. using capital expenditures
>
> ❌ Open/capex allocation, LAN changes, capacity planning
>
> > LAN change managament process don't need to change significantly
>
> ❌ Capacity planning, utilization measurement, data center expansion
>
> > Measuing utilization can be done in the same way.
> >
> > Data center expansion is not a concern for cloud customer, but taken  care of by the cloud provider.
>
> ❌ Data Center expansion, TCO calculations, utilization measurement

#### 07

To speed up data retrieval, more vehicles will be upgraded to cellular connections and be able to transmit data to the ETL process. The current FTP process is error-prone and restarts the data transfer from the start of the file when connections fail, which happens often. You want to improve the reliability of the solution and minimize data transfer time on the cellular connections. What should you do?

> ✅ Directly transfer the files to a different Google Cloud Regional Storage bucket location in US, EU and Asia using Google APIs over HTTP(S). Run the ETL process to retrieve the data from each Regional bucket
>
> > Use a region to help optimize latency and network bandwidth for data consumers, such as analytics pipelines, that are run in the same region.
> >
> > [Bucket Location](https://cloud.google.com/storage/docs/locations)
>
> ❌ Use one Google Container Engine cluster of FTP servers. Save the data to a Multi-Regional bucket. Run the ETL process using data in the bucket
>
> ❌ Use multiple Google Container Engine clusters running FTP servers located in different regions. Save the data to Multi-Regional buckets in 
>
> ❌ Directly transfer the files to different Google Cloud Multi-Regional Storage bucket locations in US, EU, and Asia using Google APIs over HTTP(S). Run the ETL process using the data in the bucket
>
> > Regional Storage is faster.

#### 08

TerramEarth's 20 million vehicles are scattered around the world. Based on the vehicle's location, its telemetry data is stored in a Google Cloud Storage (GCS) regional bucket (US, Europe, or Asia). The CTO has asked you to run a report on the raw telemetry data to determine why vehicles are breaking down after 100 K miles. You want to run this job on all the data. What is the most cost-effective way to run this job?

> ✅ Launch a cluster in each region to preprocess and compress the raw data, then move the data into a <u>region</u> bucket and use a Cloud Dataproc cluster to finish the job
>
> > + Compress the data before copying to another region/continent makes sense
> > + Preprocessing also makes sense bcuz we probably want to process smaller  chunks of data first (remember 100K milage)
> > + Multi-region is good for high-availability and low latency with a little more cost , but it's not required features per the question
>
> ❌ Move all the data into 1 zone, then launch a Cloud Dataproc cluster to run the job
>
> > No point of transferring data of the vehicles
>
> ❌ Move all the data into 1 region, then launch a Google Cloud Dataproc cluster to run the job
>
> > No point of transferring data of the vehicles
>
> ❌ Launch a cluster in each region to preprocess and compress the raw data, then move the data into a <u>multi-region</u> bucket and use a Dataproc cluster to finish the job

#### 09

TerramEarth has equipped all connected trucks with servers and sensors to collect telemetry data. Next year they want to use the data to train machine learning models. They want to store this data in the cloud while reducing costs. What should they do?

> ✅ Have the vehicle's computer compress the data in hourly snapshots, and store it in a GCS Coldline bucket
>
> > Storage is the best choice for data that you plan to access at most once a year, due to its slightly lower availability, 90-day minimum storage duration, costs for data access, and higher per-operation costs.
> >
> > + Cold Data Storage - 
> >   + Infrequently accessed data, such as data stored for legal or regulatory reasons, can be stored at low cost as Coldline Storage, and be available when you need it
> > + Disaster recovery -
> >   + In the event of a disaster recovery event, recovery time is key. Cloud Storage provides low latency access to data stored as Coldline Storage
> >
> > [Storage Classes](https://cloud.google.com/storage/docs/storage-classes)
>
> ❌ Have the vehicle's computer compress the data in hourly snapshots, and store it in a Google Cloud Storage (GCS) Nearline bucket
>
> > Nearline is more expensive than Coldline.
>
> ❌ Push the telemetry data in real-time to a streaming dataflow job that compresses the data, and store it in Google BigQuery
>
> > Storing compressed data in relational DB, which may not be possible.
> > BigQuery is more expensive than Cloud Storage Coldline.
>
> ❌ Push the telemetry data in real-time to a streaming dataflow job that compresses the data, and store it in Cloud Bigtable
>
> > Bigtable is the most expensive option, and also it's not integrated with Cloud machine learning. (Dataflow, BigQuery and Cloud Storage are integrated with ML)

#### 10

Your agricultural division is experimenting with fully autonomous vehicles. You want your architecture to promote strong security during vehicle operation. Which two architectures should you consider? (Choose two)

> ✅ Treat every micro service call between modules on the vehicle as untrusted
>
> ✅ Use a trusted platform module (TPM) and verify firmware and binaries on boot
>
> > 最小權限原則 + 信賴平台模組
>
> ❌ Require IPv6 for connectivity to ensure a secure address space
>
> > IPv6 doesn't have any impact on the security during vehicle operation, although it improves system scalability. IPv6 只提高系統可擴展性，不影響車輛營運的資安
>
> ❌ Use a functional programming language to isolate code execution cycles
>
> > Merely using a functional programming language doesn't guarantee a more secure level of execution isolation. Any impact on security from this decision would be incidental at best. 用PF寫法不保證資安等級較高的執行隔離
>
> ❌ Use multiple connectivity subsystems for redundancy
>
> > This improves system durability, but it doesn't have any impact on the security during vehicle operation. 可提高系統持久性，但不影響車輛營運的資安
>
> ❌ Enclose the vehicle's drive electronics in a Faraday cage to isolate chips
>
> > It doesn't have any impact on the security during vehicle operation, although it improves system durability. 不影響車輛營運的資安，但可以提高系統持久性

#### 11

Operational parameter such as oil pressure are adjustable on each of TerramEarth's vehicles to increase their efficiency, depending on their environmental conditions. Your primary goal is to increase the operating efficiency of all 20 million cellular and unconnected vehicles in the field. How can you accomplish this goal?

> ✅ Capture all operating data, train machine learning models that identify ideal operations, and run locally to make operational adjustments automatically
>
> > cf. option "host in Google Cloud machine learning platform to make operational adjustments"
> >
> > If we host in GCP-Machine Learning, how are we going to instruct vehicles on field to adjust their oil pressure if they have no internet connection?
> >
> > There is no way to use GCP-ML model generated parameters to command the "not connected" field vehicles to make operational adjustments automatically.
>
> ❌ Have you engineers inspect the data for patterns, and then create an algorithm with rules that make operational adjustments automatically
>
> ❌ Implement a Google Cloud Dataflow streaming job with a sliding window, and use Google Cloud Messaging (GCM) to make operational adjustments automatically
>
> ❌ Capture all operating data, train machine learning models that identify ideal operations, and host in Google Cloud Machine Learning (ML) Platform to make operational adjustments automatically

---

## TerramEarth (old_ver.2, two apps)

#### Company Overview -

TerramEarth manufactures heavy equipment for the mining and agricultural industries. About 80% of their business is from mining and 20% from agriculture. They currently have over 500 dealers and service centers in 100 countries. Their mission is to build products that make their customers more productive.

#### Solution Concept -

There are 20 million TerramEarth vehicles in operation that collect 120 fields of data per second. Data is stored locally on the vehicle and can be accessed for analysis when a vehicle is serviced. The data is downloaded via a maintenance port. This same port can be used to adjust operational parameters, allowing the vehicles to be upgraded in the field with new computing modules.

Approximately 200,000 vehicles are connected to a cellular network, allowing TerramEarth to collect data directly. At a rate of 120 fields of data per second, with 22 hours of operation per day, TerramEarth collects a total of about 9 TB/day from these connected vehicles.

#### Existing Technical Environment -

TerramEarth's existing architecture is composed of Linux and Windows-based systems that reside in a single U.S, west coast based data center. These systems gzip CSV files from the field and upload via FTP, and place the data in their data warehouse. Because this process takes time, aggregated reports are based on data that is 3 weeks old.

With this data, TerramEarth has been able to preemptively stock replacement parts and reduce unplanned downtime of their vehicles by 60%. However, because the data is stale, some customers are without their vehicles for up to 4 weeks while they wait for replacement parts.

#### Business Requirements -

- Decrease unplanned vehicle downtime to less than 1 week

- Support the dealer network with more data on how their customers use their equipment to better position new products and services
- Have the ability to partner with different companies "especially with seed and fertilizer suppliers in the fast-growing agricultural business" to create compelling joint offerings for their customers

#### Technical Requirements -

+ Expand beyond a single datacenter to decrease latency to the American midwest and east coast
+ Create a backup strategy
+ Increase security of data transfer from equipment to the datacenter
+ Improve data in the data warehouse
+ Use customer and equipment data to anticipate customer needs

#### Application 1: Data ingest -

A custom Python application reads uploaded datafile from a single server, writes to the data warehouse.

<u>Compute</u>:
Windows Server 2008 R2

+ 16 CPUs
+ 128 GB of RAM
+ 10 TB local HDD storage

#### Application 2: Reporting -

An off the shelf application that business analysts use to run a daily report to see what equipment needs repair. Only two analysts of a team of 10 (5 west coast, 5 east cost) can connect to the reporting application at a time.

<u>Compute:</u>
Off the shelf application. License tied to number of physical CPUs

+ Windows Server 2008 R2
+ 16 CPUs
+ 32 GB of RAM
+ 500 GB HDD

<u>Data warehouse:</u>
A single PostgreSQL server

- RedHat Linux
- 64 CPUs
- 128 GB of RAM
- 4x 6TB HDD in RAID 0

#### Executive Statement -

Our competitive advantage has always been in our manufacturing process, with our ability to build better vehicles for lower cost than our competitors. However, new products with different approaches are constantly being developed, and I am concerned that we lack the skills to undergo the next wave of transformations in our industry. My goals are to build our skills while addressing immediate market needs through incremental innovations.

#### 01

To be compliant with European GDPR regulation, TerramEarth is required to delete data generated from its European customers after a period of 36 months when it contains personal data. In the new architecture, this data will be stored in both Cloud Storage and BigQuery. What should you do?

> ✅ Create a BigQuery **time-partitioned** table for the European data, and set the partition expiration period to 36 months. For Cloud Storage, use gsutil to enable lifecycle management using a DELETE action with an Age condition of 36 months
>
> > When you create a table partitioned by ingestion time, BigQuery automatically loads data into daily, date-based partitions that reflect the data's ingestion or arrival time. 
> > [BigQuery: partitioned tables: ingestion_time](https://cloud.google.com/bigquery/docs/partitioned-tables#ingestion_time)
> >
> > Google recommends you configure the default table expiration for your datasets, configure the expiration time for your tables, and configure the partition expiration for partitioned tables.
> > [BigQuery: Use the expiration settings to remove unneeded tables and partitions](https://cloud.google.com/bigquery/docs/best-practices-storage#use_the_expiration_settings_to_remove_unneeded_tables_and_partitions)
> >
> > If the partitioned table has a table expiration configured, all the partitions in it are deleted according to the table expiration settings. For our specific requirement, we could se the partition expiration to 36 months so that partitions older than 36 months (and the data within) are automatically deleted.
> > [BigQuery: set the partition expiration](https://cloud.google.com/bigquery/docs/managing-partitioned-tables#partition-expiration)
>
> ❌ Create a BigQuery table for the European data, and set the table retention period to 36 months. For Cloud Storage, use gsutil to enable lifecycle management using a DELETE action with an Age condition of 36 months
>
> ❌ Create a BigQuery table for the European data, and set the table retention period to 36 months. For Cloud Storage, use gsutil to create a SetStorageClass to NONE action when with an Age condition of 36 months
>
> ❌ Create BigQuery time-partitioned table for the European data, and set the partition expiration period to 36 months. For Cloud Storage, use gsutil to create a SetStorageClass to NONE actions with an Age condition of 36 months

#### 02

TerramEarth has decided to store data files in Cloud Storage. You need to configure Cloud Storage lifecycle rule to store 1 year of data and minimize file storage cost. Which two actions should you take?

> ✅ (1) Create a Cloud Storage lifecycle rule with -- 
>               Age:[30], 
>               Storage Class: [Standard]
>               Action: [Set to Coldline]
>       (2) Create a second GCS life-cycle rule with --
>               Age:[365], 
>               Storage Class: [Coldline]
>               Action: [Delete]
>
> > Two options talk about deleting the file after one year, but incorrect "Nearline"
>
> ❌ (1) Create a Cloud Storage lifecycle rule with -- 
>               Age:[30], 
>               Storage Class: [Coldline]
>               Action: [Set to Nearline]
>       (2) Create a second GCS life-cycle rule with --
>               Age:[91], 
>               Storage Class: [Coldline]
>               Action: [Set to Nearline]
>
> ❌ (1) Create a Cloud Storage lifecycle rule with -- 
>               Age:[90], 
>               Storage Class: [Standard]
>               Action: [Set to Nearline]
>       (2) Create a second GCS life-cycle rule with --
>               Age:[91], 
>               Storage Class: [Nearline]
>               Action: [Set to Coldline]
>
> ❌ (1) Create a Cloud Storage lifecycle rule with -- 
>               Age:[30], 
>               Storage Class: [Standard]
>               Action: [Set to Coldline] *
>       (2) Create a second GCS life-cycle rule with --
>               Age:[365], 
>               Storage Class: [Nearline] *
>               Action: [Delete]

#### 03

You need to implement reliable, scalable GCP solution for the data warehouse for your company, TerramEarth. Considering the TerramEarth business and technical requirements, what should you do?

> ✅ Replace the existing data warehouse with BigQuery. Use table partitioning.
>
> > The reliable way of improving the data warehouse is to have a table partitioned and well managed
> >
> > BigQuery supports partitioning tables by date. You enable partitioning during table-creation process.
> > BigQuery creates new date-based partitions automatically, with no need for additional maintenance.
> > Also, you can specify an expiration time for data in the partitions.
>
> ❌ Replace the existing data warehouse with a Compute Engine instance with 96 CPUs
>
> ❌ Replace the existing data warehouse with BigQuery. Use federated data sources
>
> > Federated is an option but not a reliable option.
> >
> > You can run queries on data that exists outside of BigQuery by using *federated data sources*, but this approach has performance implications. Use federated data sources only if the data must be maintained externally. You can also use query federation to perform ETL from an external source to BigQuery. This approach allows you to define ETL using familiar SQL syntax.
>
> ❌ Replace the existing data warehouse with a Compute Engine instance with 96 CPUs. Add an additional Compute Engine preemptible instance with 32 CPUs

#### 04

A new architecture that writes all incoming data to BigQuery has been introduced. You notice that the data is dirty, and want to ensure data quality on an automated daily basis while managing cost. What should you do?

> ✅ Use Cloud Dataprep and configure the BigQuery tables as the source. Schedule a daily job to clean the data
>
> > **Dataprep** has the capabilities to clean dirty data. [Dataprep](https://cloud.google.com/dataprep)
> >
> > + Good for fast exploration and anomaly detection
> > + Support scheduling.
> >   When the scheduled job successfully executes, you can collect the wrangled output in the specified location
> > + Integrates naturally with BigQuery (and Cloud Storage, for directly uploading file) and it uses Dataflow under the hood
>
> ❌ Set up a streaming Cloud Dataflow job, receiving data by the ingestion process. Clean the data in a Cloud Dataflow pipeline
>
> ❌ Create a Cloud Function that reads data from BigQuery and cleans it. Trigger the Cloud Function from a Compute Engine instance.
>
> > Cloud Dataflow and Cloud Function are about real-time processing, which is not needed (per requests 'on daily basis').
> >
> > Alos to check if data is dirty, you may analyze several adjacent rows, so real-time processing may not physically solve a problem.
>
> ❌ Create a SQL statement on the data in BigQuery, and save it as a view. Run the view daily, and save the result to a new table.

#### 05

Considering the technical requirements, how should you reduce the unplanned vehicle downtime in GCP?

> ✅ Use BigQuery as the data warehouse. Connect all vehicles to the network and stream data into BigQuery using Cloud Pub/Sub and Cloud Dataflow. Use Google Data Studio for analysis and reporting
>
> > Stream data with managed services approach
> >
> > [Connected device architectures on Google Cloud](https://cloud.google.com/architecture/connected-devices#data_ingestion)
>
> ❌ Use BigQuery as the data warehouse. Connect all vehicles to the network and upload gzip files to a Multi-Regional Cloud Storage bucket using cloud. Use Google Data Studio for analysis and reporting
>
> ❌ Use Cloud Dataproc Hive as the data warehouse. Upload gzip files to a Multi-Regional Cloud Storage bucket. Upload this data into BigQuery using cloud. Use Google Data Studio for analysis and reporting
>
> ❌ Use Cloud Dataproc Hive as the data warehouse. Directly stream data into partitioned Hive tables. Use Pig scripts to analyze data.

#### 06

You are asked to design a new architecture for the ingestion of the data of the 200,000 vehicles that are connected to a cellular network. You want to follow Google-recommended practices. Considering the technical requirements, which components should you use for the ingestion of the data?

> ✅ Cloud **IoT Core** with public/private key pairs
>
> > IoT Core: 
> >
> > - is developed for connecting existing devices spread around the world to GCP.
> > - supports end-to-end security using asymmetric key authentication over TLS 1.2
>
> ❌ Google Kubernetes Engine with an SSL Ingress
>
> ❌ Compute Engine with project-wide SSH keys
>
> ❌ Compute Engine with specific SSH keys

---

## TerramEarth

#### Company overview

TerramEarth manufactures heavy equipment for the mining and agricultural industries. They currently have over 500 dealers and service centers in 100 countries. Their missions is to build products that make their customers more productive. --- 農業/礦業工具機製造商，全世界在100個國家有服務據點與500個經銷商.

#### Solution concept

There are 2 million TerramEarth vehicles in operation currently, and we see 20% yearly growth. Vehicles collect telemetry data from many sensors during operations. A small subset of critical data is transmitted from the vehicles in real time to facilitate fleet management. The rest of the sensor data is collected, compressed, and updated daily when the vehicles return to home base. Each vehicle usually generates 200 to 500 megabytes of data per day.

#### Existing technical environment

TerramEarth's vehicle data aggregation and analysis infrastructure resides in Google Cloud and serves clients from all around the world. 資料整合與分析的基礎建設在GoogleCloud，客戶遍佈全世界. A growing amount of sensor data is captured from their two main manufacturing plants and sent to private data centers that contain their legacy inventory and logistics management systems. The private data centers have multiple network interconnects configured to Google Cloud.

The web frontend for dealers and customers is running in Google Cloud and allows access to stock management and analytics.

#### Business requirements

+ Predict and detect vehicle malfunction and rapidly ship parts to dealerships for just-in-time repair where possible. 預測/偵測車輛故障，迅速運送到經銷商做即時維修
+ Decrease cloud operational costs and adapt to seasonality 減少雲操作成本 適應淡旺季(嗎？)
+ Increase speed and reliability of development workflow
+ Allow remote developers to be productive without compromising code or data security
+ Create a flexible and scalable platform for developers to create custom API services for dealers and partners

#### Technical requirements

+ Create a new abstraction layer for HTTP API access to their legacy systems to enable a gradual move into the cloud without disrupting operations. 建立HTTP API抽象層存取既有系統, 上雲不會干擾實際作業
+ Modernize all CI/CD pipelines to allow developers to deploy container-based workloads in highly scalable environments. 優化CICD，讓開發人員能在高擴張性環境下，部署容器化的工作負載
+ Allow developers to run experiments without compromising security and governance requirements
+ Create a self-service portal for internal and partner developers to create new projects, request resources for data analytics jobs, and centrally manage access to the API endpoints
+ Use cloud-native solutions for keys and secrets management and optimize for identity-based access
+ Improve and standardize tools necessary for application and network monitoring and troubleshooting

#### Executive statement

Our competitive advantage has always been our focus on the customer, with our ability to provide excellent customer service and minimize vehicle downtimes. 

After moving multiple system into Google Cloud, we are seeking new ways to provide best-in-class online fleet management (車隊管理) services to our customers and improve operations of our dealerships (代理權/經銷權). Our 5-year strategic plan is to create a partner ecosystem of new products by enabling access to our data, increasing autonomous operation capabilities of our vehicles, and creating a path to move the remaining legacy systems to the cloud.

#### 01

You are the data compliance officer for TerramEarth and must protect customers' personally identifiable information (PII), like credit card information. TerramEarth wants to personalize product recommendations for its large industrial customers. You need to respect data privacy and deliver a solution. What should you do?

> ✅ Use the Cloud Data Loss Prevention (DLP) API to provide data to the recommendation service. 
>
> > Cloud DLP --- was specifically designed for this use case
>
> ❌ Use AutoML to provide data to the recommendation service 
>
> > Incorrect, as AutoML does not inherently provide data de-identification
>
> ❌ Process PII data on-premises to keep the private information more secure
>
> > Incorrect, as TerramEarth's requirements are to go into the cloud, not stay on-premises
>
> ❌ Manually build, train, and test machine learning models to provide product recommendations anonymously 
>
> > Incorrect, as developing machine learning models is an excessive way to de-identify data.

#### 02



> ✅
>
> ❌
>
> ❌
>
> ❌

03





> ✅
>
> ❌
>
> ❌
>
> ❌

04





> ✅
>
> ❌
>
> ❌
>
> ❌

05





> ✅
>
> ❌
>
> ❌
>
> ❌

06





> ✅
>
> ❌
>
> ❌
>
> ❌

07





> ✅
>
> ❌
>
> ❌
>
> ❌

---

Dress4Win Questions* 11



---

Dress4Win Questions* 6



---

JencoMart Question* 6



---

## Helicopter Racing League

#### Company overview

Helicopter Racing League (HRL) is a global sports league for competitive helicopter racing. Each year HRL holds the world championship and several regional league competitions where teams compete to earn a spot in the world championship. HRL offers a paid service to stream the races all over the world with live telemetry and predictions throughout each race.

#### Solution concept

HRL wants to migrate their existing service to a new platform to expand their use of managed AI and ML services to facilitate race predictions (make predicting easier). Additionally, as new fans engage with the sport, particularly in emerging regions, they want to move the serving of their content, both real-time and recorded, closer to their users.

1. 利用AI＆機器學習讓賽事預測變簡單  
2. 將即時/預錄賽事資料移到離新興收看區域比較近的地方

#### Existing technical environment

HRL is a public cloud-first company; the core of their mission-critical applications runs on their current public cloud provider. Video recording and editing is performed at the race tracks, and the content is encoded and transcoded, where needed, in the cloud. 在賽道上錄影/剪輯影片、雲端上視訊編碼/轉碼

Enterprise-grade connectivity and local compute is provided by truck-mounted mobile data centers. Their race prediction services are hosted exclusively on their existing public cloud provider. Their existing technical environment is as follows:

+ Existing content is stored in an object storage service on their existing public cloud provider.
+ Video encoding and transcoding is performed on VMs created for each job.
+ Race predictions are performed using TensorFlows running on VMs in the current public cloud provider

#### Business requirements

HRL's owners want to expand their predictive capabilities and reduce latency for their viewers in emerging markets. Their requirements are:

+ Support ability to expose the predictive models to partners
+ Increase predictive capabilities during and before races:
  + Race results 
  + Mechanical failures 
  + Crowd sentiment
+ Increase telemetry(遙測技術) and create additional insights 
+ Measure fan engagement with new predictions
+ Enhance global availability and quality of the broadcasts
+ Increase the number of concurrent viewers 提高線上收看人數
+ Minimize operational complexity
+ Ensure compliance with regulations
+ Create a merchandising revenue stream 建立商品成交金流

#### Technical requirements

+ Maintain or increase prediction throughput and accuracy 
  維持或提高預測吞吐量以及準確性
+ Reduce viewer latency 
  降低觀看延遲
+ Increase transcoding performance 
  提高轉碼 (視訊-音訊) 效能
+ Create real-time analytics of viewer consumption patterns and engagement 
  建立觀眾消費行為與互動的即時分析
+ Create a data mart to enable processing of large volumes of race data 
  建立可處理大量賽事資料的資料市集 
  資料市集---專注於特定主題或營業單位、為簡單形式的資料倉儲 ([參考文章](https://aws.amazon.com/compare/the-difference-between-a-data-warehouse-data-lake-and-data-mart/?nc1=h_ls))

#### Executive statement

Our CEO, S. Hawke, wants to bring high-adrenaline racing to fans all around the world. We listen to our fans, and they want enhanced video streams that include <u>predictions of events within the race</u> (e.g. overtaking). Our current platform allows us to predict race outcomes but laks the facility to support real-time predictions during races and the capacity to process season-long results.

> 需求：在賽事進行中的即時預測＆處理整季度比賽結果的能力

#### 01

Your team is in charge of creating a payment card data vault for card numbers used to bill tens of thousands of viewers, merchandise consumers, and season ticket holders. You need to implement a custom card tokenization service that meets the following requirements:

- It must provide low latency at minimal cost
- It must be able to identify duplicate credit cards and must not store plaintext card numbers
- It should support annual key rotation

Which storage approach should you adopt for your tokenization service?

> ✅ Encrypt the card data with a deterministic algorithm (and) stored in Firestore using Datastore mode
>
> > Deterministic output: A given set of inputs (card number, expiration, userID) will always generate the same token. This is useful if you want to rely on the token value to de-duplicate your token stores. You can simply match a newly generated token to your existing catalog of tokens to determine whether the card has been previously stored.
> >
> > Depending on your application, this can be a very useful feature, but this could also be accomplished using a salted hash of the input values.
> >
> >  [Credit Card Tokenization: PCI tokenizer](https://cloud.google.com/community/tutorials/pci-tokenizer)
> >
> > Firestore is the next major version of Datastore. Firestore can run in Datastore mode, which uses the same API as Datastore and scales to millions of writes per second.
> >
> > [Tokenizing sensitive cardholder data for PCI DSS](https://cloud.google.com/architecture/tokenizing-sensitive-cardholder-data-for-pci-dss)
>
> ❌ Store the card data in Secret Manager after running a query to identify duplicates
>
> ❌ Encrypt the card data with a deterministic algorithm and shard it across multiple Memorystore instances
>
> ❌ Use column-level encryption to store the data in Cloud SQL

#### 02

Recently HRL started a new regional racing league in Cape Town, South Africa. In an effort to give customers in Cape Town a better user experience, HRL has partnered with the Content Delivery Network provider, Fastly. HRL needs to allow traffic coming from all of the Fastly IP address ranges into their Virtual Private Cloud network (VPC network). You are a member of the HRL security team and you need to configure the update that will allow only the Fastly IP address ranges through the External HTTP(S) load balancer. Which command should you use?

> ```terminal
> ✅
> gcloud compute security-policies rules update 1000
>     --security-policy hlr-policy
>     --expression "evaluatePreconfiguredExpr('sourceiplist-fastly')"
>     --action "allow"
> ```
>
> > 1. They want to apply a firewall rule at HTTPS load balancing level. This can be done only with cloud armor. Cloud armor doesn't work with classic firewall; it works with security policies.
> > 2. Above option is the only answer that points to a list of IPs to whitelist, via <u>preconfigured expression</u>. The others will open to too broad IPs.
> >
> > [Configure a secure policy](https://cloud.google.com/armor/docs/configure-security-policies#gcloud_11)
>
> ```terminal
> ❌
> gcloud compute security-policies rules update 1000
>     --security-policy from-fastly
>     --src-ip-ranges *
>     --action "allow"
> ```
>
> ```terminal
> ❌
> gcloud compute firewall rules update sourceiplist-fastly
>     --priority 1000
>     --allow tcp:443  
> ```
>
> ```terminal
> ❌
> gcloud compute firewall rules update hlr-policy
>     --priority 1000
>     --target-tags=sourceiplist-fastly
>     --allow tcp:443
> ```

#### 03

The HRL development team releases a new version of their predictive capability application every Tuesday evening at 3 a.m. UTC to a repository. The security team at HRL has development an in-house penetration test Cloud Function called Airwolf. The security team wants to run Airwolf against the predictive capability application as soon as it is released every Tuesday. You need to set up Airwolf to run at the recurring weekly cadence. What should you do?

> ✅ Configure the deployment job to notify a Pub/Sub queue that triggers a Cloud Function
>
> > Trigger pub/sub --- to invoke Cloud Functions
> > Cloud Storage --- no sense
> > 
>
> ❌ Set up Cloud Tasks and a Cloud Storage bucket that triggers a Cloud Function
>
> ❌ Set up a Cloud Logging sink and a Cloud Storage bucket that triggers a Cloud Function
>
> ❌ Set up Identity and Access Management (IAM) and Confidential Computing to trigger a Cloud Function

04



> ✅
>
> ❌
>
> ❌
>
> ❌

05



> ✅
>
> ❌
>
> ❌
>
> ❌

06



> ✅
>
> ❌
>
> ❌
>
> ❌



---

# Non-case-study

#### 01

You are designing a future-proof hybrid environment that will require network connectivity between Google Cloud and your on-premises environment. You want you ensure that the Google Cloud environment you are designing is compatible with your on-premises networking environment. What should you do?

> ☀︎ Create a network plan for your VPC in Google Cloud that uses non-overlapping CIDR ranges with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud. ☀︎
> --- Correct, as this ensures your on premises network is compatible with your Google Cloud VPC.
>
> Use the default VPC in your Google Cloud project. Use a Cloud VPN connection between your on-premises environment and Google Cloud --- Incorrect, as the default VPC is a VPC with Auto Mode IP ranges, which has the same problem as answer C
>
> Create a custom VPC in Google Cloud auto mode. Use a Cloud VPN connection between your on-premises environment and Google Cloud --- Incorrect, because with Auto Mode IP ranges there is no guarantee that the IP ranges will not overlap with your on premises environment, either now or in the future. 
>
> Create a network plan for your VPC in Google Cloud that uses CIDR ranges that overlap with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud --- Incorrect, because to ensure correct routing, ranges cannot overlap between environments. 

#### 02

Your company wants to track whether someone is present in a meeting room reserved for a scheduled meeting. There are 1000 meeting rooms across 5 offices on 3 continents. Each room is equipped with a motion sensor that reports its status every second. You want to support the data ingestion needs of this sensor network. The receiving infrastructure needs to account for the possibility that the devices may have inconsistent connectivity. Which solution should you design?

> ☀︎ Have devices poll for connectivity to Pub/Sub and publish the latest messages on a regular interval to a shared topic for all devices ☀︎
>
> Pub/Sub --- Correct, as it can handle the frequency of this data, and consumers of the data can pull from the shared topic for further processing.
>
> Have each device create a persistent connection to a Compute Engine instance and write messages to a custom application --- Incorrect, as having a persistent connection does not handle the case where the device is disconnected.
>
> Have devices poll for connectivity to Cloud SQL and insert the latest messages on a regular interval to a device specific table --- Incorrect, as Cloud SQL is a regional, relational database and not the best fit for sensor data. Additionally, the frequency of the writes has the potential to exceed the supported number of concurrent
>
> Have devices create a persistent connection to an App Engine application fronted by Cloud Endpoints, which ingest messages and write them to Datastore --- Incorrect, as having a persistent connection does NOT handle the case where the device is disconnected.

#### 03

Your company wants to try out the cloud with low risk. They want to archive approximately 100TB of their log data to the cloud and test the serverless analytics features available to them there, while also retaining that data as a long-term disaster recovery backup. Which two steps should they take? (Choose two)

> ✅ Load logs into BigQuery
>     Correct, as BigQuery is a serverless cloud data warehouse for analytics and supports the volume and analytics requirement (無伺服器/支援量/支援分析)
>
> ✅ Upload log files into Cloud Storage
>     Correct, as Cloud Storage provides the Coldline and Archive storage classes to support long-term storage with infrequent access, which would support the long-term disaster recovery backup requirement (備份用, coldline / archive - 90天/一年存取一次)
>
> ❌ Log logs into Cloud SQL 
>     Incorrect, as it does not support the expected 100 TB. Additionally, Cloud SQL is a relational database and not the best fit for time-series log data formats (Cloud SQL 不支援到 100 terabytes 且關聯式資料庫不是時間序列數據格式的最佳選擇)
>
> ❌ Import logs into Cloud Logging
>     Incorrect, as Cloud Logging is optimized for monitoring, error reporting, and debugging instead of analytics queries (Cloud Logging適用：監測、回報錯誤、除錯, 不用在分析查詢)
>
> ❌ Insert logs into Cloud Bigtable
>     Incorrect, as Cloud BigTable is optimized for read-write latency and analytics throughput, not analytics querying and reporting (BigTable優勢：讀寫延遲性 以及 分析高吞吐量, 不適合查詢分析/產製報表)

#### 04

You set up an autoscaling managed instance group to serve web traffic for an upcoming launch. After configuring the instance group as a backend service to an HTTP(S) load balancer, you notice that virtual machine (VM) instances are being terminated and re-launched every minute. The instances do not have a public IP address. You have verified that the appropriate web response is coming from each instance using the curl command. You want to ensure that the backend is configured correctly. What should you do?

> ✅ Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group 
>     Correct, as health check failures lead to a VM being marked unhealthy and can result in termination if the health check continues to fail. Because you have already verified that the instances are functioning properly, the next step would be to determine why the health check is continuously failing.
>
> ❌ Ensure that a firewall rule exists to allow source traffic on HTTP/HTTPS to reach the load balancer
>     Incorrect, as the issue to resolve is the VMs being terminated, not access to the load balancer.
>
> ❌ Assign a public IP to each instance, and configure a firewall rule to allow the load balancer to reach the instance public IP
>     Incorrect, as this introduce a security vulnerability without addressing the primary concern of the VM termination.
>
> ❌ Create a tag on each instance with the name of the load balancer. Configure a firewall rule with the name of the load balancer as the source and the instance tag as the destination
>     Incorrect, as the source of the firewall rule that allows load balancer and health check access to instances is defined IP ranges, and not a named load balancer. Tagging the instances for the purpose of firewall rules is appropriate but would probably be a descriptor of the application, and not the load balancer.

#### 05

Your organization has a 3-tier web application deployed in the same Google Cloud Virtual Private Cloud (VPC). Each tier (web, API, and database) scales independently of the others. Network traffic should flow through the web to the API tier, and then on to the database tier. Traffic should not flow between the web and the database tier. How should you configure the network with minimal steps?

> ✅ Add tags to each tier and set up firewall rules to allow the desired traffic flow
>     Correct, because as instances scale, they will all have the same tag to identify the tier. These tags can then be leveraged in firewall rules to allow and restrict traffic as required, because tags can be used for both the target and source.
>
> ❌ Add each tier to a different subnetwork
>     Incorrect, as the subnetwork alone will not allow and restrict traffic as required without firewall rules.
>
> ❌ Set up software-based firewalls on individual VMs
>
> ​    Incorrect, as this adds complexity to the architecture and the instance configuration.
>
> ❌ Add tags to each tier and set up routes to allow the desired traffic flow
>     Incorrect, as routes still require firewall rules to allow traffic as requests. Additionally, the tags are used for defining the instances the route applies to, and not for identifying the next hop. The next hop is either an IP range or instance name, but in the proposed solution the tiers are only identified by tags.

#### 06

You are designing a large distributed application with 30 microservices. Each of your distributed microservices needs to connect to a database backend. You want to store the credentials securely. Where should you store the credentials?

> ✅ In a secret management system
>     Correct, as a secret management system such as Secret Manager is a secure and convenient storage system for API keys, passwords, certificates, and other sensitive data. Secret Manager provides a central place and single source of truth to manage, access and audit secrets across Google Cloud.
>
> ❌ In the source code
>     Incorredt, as storing credentials in source code and source control is discoverable, in plain text, by anyone with access to the source code. This also introduces the requirement to update code and do a deployment each time the credentials are rotated.
>
> ❌ In an environment variable
>     Incorrect, as consistently populating environment variables would require the credentials to be available, in plain text, when the session is started.
>
> ❌ In a config file that has restricted access through ACLs
>     Incorrect, bcuz instead of managing access to the config file and updating manually as keys are rotated, it would be better to leverage a key management system. Additionally, there is increased risk if the config file contains the credentials in plain text.

#### 07

Your customer is moving their corporate applications to Google Cloud. The security team wants detailed visibility of all resources in the organization. You use Resource Manager to set yourself up as the Organization Administrator. Which Identity and Access Management (IAM) roles should you give to the security team while following Google recommended practices?



08



09

