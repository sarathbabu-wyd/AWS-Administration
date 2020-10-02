# Cloud Computing
   
 * Cloud Computing is the on-demand delivery of compute power,database storage,applications,and other IT resouces.
 * Through a cloud services platform with pay-as-you-go pricing.
 * provision of right type and size.(Bigone,smallone,ten,two for tomorrow)
 * Easy access to servers,storage,databases and set of application services.
 * Deployment models of Cloud
   
![CLOUD_PRACTITIONER](cloud_deployment_models.png)

## Types of Cloud Computing

 1. Infrastucture as a Service (IaaS)
 >> Provide building blocks for cloud IT
 >> Provides networking,computers,data storage space
 >> Highest level of flexibility
 >> easy parallel with traditional on-premises 
 >> Eg:Amazon EC2,Azure,Rackapace,GCP

 2. Platform as a Service (PaaS)
 >> Removes the need for your organization to manage the underlying infrastucture
 >> Focos on the deployment and management of your applications
 >> Eg:Elastic Beanstalk (on AWS),Google App Engine(GCP)

 3. Softwrae as a Service (SaaS)
 >> Completed  product that is run and managed by the service provider
 >>Eg:Many AWS services(Rekognition for machine learning),Google Apps (Gmai,Dropbox)

   ![CLOUD_PRACTITIONER](Compare_services_Onpremises.png)

### Pricing Fundamentals

1. Compute(pay for compute time)
2. Storage (pay for data stored in cloud)
3. Pay Data transfer OUT of the Cloud(Data Transfer IN is Free)

### AWS Global Structure

* The AWS Global infrastucture is designed and built to deliver the most flexible,reliable,scalable, and secure cloud computing environment with the highest quality global network performance available today.AWS have 230 Allowed services.www.infrastucture.aws

1. Regions -North America(Oregon,N.california,ohio ,etc),South America(sao paulo),Europe(ireland,london,spain,milan,paris,frankfurt,stockholm) and Asia pacific(baharin,mumbai,singapore,tokyo,sydney, etc).Total of 26 regions all around the world

2. Availability Zones(AZ)-Each regions have min 2 to max 6 availability Zones.AWS maintains 77 availability Zones around the world.Each AZ can be multiple data centres and at full scale have thousands of servers.AZs are seprated from one another by many km.
>>Data centres are key to AWS infrastucture need to protect from manmade and natural risks.It has four layers PERIMETER LAYER(security gurds,fencing), INFRASTUCTURE LAYER(backup,HVAC,fire safety), DATA lAYER(area holds customer data,authorised person have only access), ENVIRONMENTAL LAYER(site selection depends on flood ,weather)

3. Local Zones -its aws new deployment place near large poplation,industry and IT centres.Its an Extension of AWS region ,and can run services a)Amazon Elastic Compute Cloud, b)Amazon Virtual private cloud. c)Amazon elastic Block store, d)Amazon File storage, e)Amazon Elastic Load Balancing

4. Point of Presence(PoP)-Amazon has 216 Points of presence in 84 cities .Deliver customer content through worldwide network of PoP, consists of Edge locations and Regional Edge cache servers. Amazon Cloudfront is a fast Content Delivery Network (CDN) thar securely delivers data, videos ,application with high transfer speed.

![CLOUD_PRACTITIONER](Shared_responsibility_model.png)
![CLOUD_PRACTITIONER](shared_respo_model2.png)


# IAM -IDENTITY AND ACCESS MANAGEMENT

* Global service
* Root account created by default shouldn't be shared
* Users within your oraganization can be grouped
* Groups only contain users,not other groups
* User don't have to belong to a group,and user can belong to multiple groups
* @ follwed by name indicates IAM user

## IAM Roles

* EC2 instance Roles
* Lambda Function Roles
* Roles for Cloud Formation

# AMAZON EC2

* EC2 is one of the popular of AWS offering
* EC2 = Elastic Compute Cloud = Infrastucture as a Service
* its mainly consists of
  >>Renting  virtual machine (EC2)
  >>Storing data on virtual drives (EBS,elastic block store)
  >>Distributing Load across machines (ELB,elastic load balancing)
  >>Scaling the services using auto-scaling group (ASG)
* Knowing EC2 is fundamental to understand how the Cloud   works

## EC2 SiZING AND CONFIG
* operating system-Linux or Windows
* How much compute powers and core (CPU)
* How much random-access memory(RAM)
* How much storage space:
  >> Network-attached(EBS & EFS)
  >> hardware (EC2 Instance Store)
* Network card:speed of the card,Public IP address
* Firewall rules:security group
* Bootstrap script(configure at first launch):EC2 User    data 

![CLOUD_PRACTITIONER](EC2_instant_types.png)

## SECURITY GROUPS

 * Security Groups are fundamentals of network security in AWS
 * They control how traffic is allowed into or out of our EC2 instances.
 * Security groups only contain allow rules
 * Security groups rules can reference by IP or by security group

### SECURITY GROUPS -DEEPER DRIVE

* Security groups are acting as a "Firewall" on EC2 instances
* They regulate
  >> Access to Ports
  >> Authorised IP ranges -IPv4 and IPv6
  >> Contol of inbound network
  >> Control of outbound network

## CLASSIC PORTS

* 22 = SSH (Secure Shell)-login into linux instance
* 21 = FTP (File Transport Protocol)
* 22 = SFTP (Secure File Transport Protocol)
* 80 = HTTP -access unsecured websites
* 443 = HTTPS -access secured websites
* 3389 = RDP (Remote Desktop Protocol)-log into a Windows instance 

## EC2 INSTANCE PURCHASING OPTIONS

* ON-DEMAND INSTANCES(pay for what you use,has highest cost but no upfront payment,no long term commitment)

* RESERVED INSTANCES-(Minimum 1 year,75% discount as compared to on-demand,)
 >> purchasing options:no upfront=no discount,partial upfront=+ discount,all upfront=++ discount
 >> reserve a specific instance type
 1.convertible reserved instances(can change EC2 type,up to 54% of discount)
 2.scheduled reserved instances(commitment for 1-3 years,require a fraction of day/week/month, EG:every thursday between 3 and 6 pm)

* EC2 SPOT INSTANCES-90% discount as compared to on-demand,can lose at any time if your max price is less than current spot price,the most cost-efficient

* EC2 DEDICATED HOSTS-
 >>An amazon EC2 dedicated host is a physical server with EC2 instance fully dedicated to you use.
 >>Allocated for your account for a 3 year period reservation
 >> More expensive
 >> for companies that have strong regulatory or compliance needs.  

![CLOUD_PRACTITIONER](purchse-options_sample.png)
![CLOUD_PRACTITIONER](Shared_respo_EC2model.png)

## EC2 STORAGE -EBS(ELASTIC BLOCK STORE)

* An EBS Volume is a network drive you can attach to your instances while they run.
* They can be mounted to one instance at a time.
* They are bound to a specific availability zone.
* Analogy:Think them as "network USB" stick
* Free tier: 30GB of free EBS stoarge of type gp2 per month
* EBS volume-
  >>it uses network to communicate,might be a bit of latency
  >>it can be detached from EC2 instance and attached to another one quickly.
  >>An EBS volume in us-east-1a cannot be attached to us-east-1b
  >>to move volume across you first need to snapshot it.
  >>You can increase the capacity of the drive over time
  >>you get billed for all provisional capacity(size in GBs and IOPS-input output operations per second)
### We cannot attach an EBS volume to two EC2 instances but possible us to attach two EBS volumes to one instances.

### EBS SNAPSHOTS

* Make a backup (snapshot) of your EBS volume at a point in time
* Not necessary to detach volume to do snapshot,but recommended
* Can copy snap shot across AZ or Regions

## AMI -AMAZON MACHINE IMAGE

1. AMI are customization of an EC2 instance
  >>you add you own software ,configuration,operating system ,monitoring..
  >>Faster boot/configuration time because all your software is pre-packaged
2. AMI are built for a specific region and can be copied across regions.
3. You can launch EC2 instances from :
   >>A public AMI -AWS provided
   >>Your owm AMi -You make and maintain them youself
   >>An AWS Marketplace AMI:an AMI someone else made   

## EC2 INSTANCE STORE

* EBS volumes are network drives with good but limited performance
* If you need a high-performance hardware disk,Use EC2 instance store
* Better I/O performance
* EC2 instance store lose their data if they are stopped
* Risk of data loss if hardware fails. 

## EFS-ELASTIC FILE SYSTEM

 * Managed NFS(network file system) that can be mounted on 100s of EC2
 * EFS works with Linux EC2 instances in multi AZ
 * Highly available,scalable,expensive(3xgp2),per pay use.

 ![CLOUD_PRACTITIONER](EBS_vs_EFS.png)
 ![CLOUD_PRACTITIONER](Sharedrespo_EC2_storage.png)

 ## ELB & ASG(ELASTIC LOAD BALANCING & AUTO SCALING GROUP)

 ### SCALABILITY AND HIGH AVAILABILITY
   * Scalability means that an application / system can handle greater loads by adapting.
   * There are two kinds scalability
     >>Vertical scalability-Increase instance size(=scale up/ down).From:t2.nano-0.5G of RAM,1 vcpus --TO : u-12tbl.metal-12.3TB of RAM,448 vCPUs
     >>Horizontal scalability(=elasticity)-Increase number of instances (=scale out/in).it consists of
       1.Auto Scaling Group
       2.Load Balancer

    * High availability means running application /system in at least 2 Availability Zones. 
     >>it goes hand in hand with horizontal scaling. 
     >>The goal is to survive data center loss(disaster)
     ELASTICITY-In scalable system elasticity means that there will be some "auto -scaling" so that the system can scale based on the load.This is "cloud-friendly":pay-per-use,match demand,optimize costs.

  ### ELB
     * Load balancers are servers that forward internet traffic to multiple server to multiple servers(EC2 instances)
     * Expose single point of access(DNS) to your application
     * Provide HTTPS for your websites
     * High availability zones
     * An ELB ia a managed load balancer
       1. AWS guarantees that it will be working.    
       2. AWS takes cares of upgrades,maintenance,high availability
       3. AWS provides only a few configuration knobs.
     * Its costs less but high effort on maintenance
     * 3 kinds of Load Balancer offered by AWS:
       1. Application Load Balancer(HTTP/HTTPS only)-Layer 7
       2. Network load Balnacer(ultra high performance,allows TCP)-Layer 4
       3. Classic Load Balancer-layer 4 & 7 


   ### AUTO SCALING GROUP
      * In real-life the cloud on your websites and application vcan change
      * The Goal of ASG is:
        1. Scale out(add EC2 instances) to match an increased load
        2. Scale in(remove EC2 instances) to match decreases load
        3. Ensure we have minimum and maximum number of machines running.
        4. Automatically register new instances to a load balancer.
        5. Replace unhealthy instances
      * Cost saving : only run at optimal capacity  

