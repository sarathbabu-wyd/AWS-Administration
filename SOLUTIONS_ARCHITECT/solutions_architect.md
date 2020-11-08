# AWS FUNDAMENTALS (IAM & EC2)

## SSH COMMAND (To login to another machine)
* ssh -i (path of pem) ec2-user@Ip

## EC2 INSTANCE CONNECT (   another way to login)
* Click on EC2 instance---click connect---click on EC2 instance connect----connect

## SECURITY GROUPS DEEP DRIVE

* Can be attached to multiple instances
* Locked down to a region.If you want to switch to another region you want to create a new security group/VPC
* The security group live outside the EC2.If traffic is blocked EC2 instances won't even see it.
* Its good to maintain one separate security group for SSH access.
* If your application is not accessible ,then its security group issue

## PRIVATE VS PUBLIC IP (IPV4)

* PUBLIC IP
1. Public IP means the machine can be identified on the internet(www)
2. Must be unique across the whole web (no two machines can have same public IP)
3. Can be geo-loacated easily.

* PRIVATE IP
1. Private IP means the machine can be only be identified on a private network only.
2. The IP must be unique across the private network.
3. BUT two different private networks(two companies) can have same IPs
4. Machines connect to WWW using an internet Gateway(a proxy)
5. Only a specified range of IPs can be used us private IP

* ELASTIC IPs

1. When you stop and then start an EC2 instance,it can change its public IP.
2. If you need to have a fixed public IP for your instance,you need an Elastic IP.
3. An Elastic Ip is a public IPv4 you own as long as you dont't delete it.
4. You can attach it to one instance at a time
5. With an Elastic IP address you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
6. You can only have 5 Elastic IP in your account

WARNING UNPROTECTED PRIVATE KEY FILE!
sudo chmod 600  /home/sarath/Downloads/EC2Tutorial.pem

## INSTALL APACHE ON EC2

* sudo apt-get update
* sudo apt-get install httpd.x86_64
* systemctl enable httpd.service 
* curl localhost :80 
* echo "Hello World from $(hostname -f" > ?var/www/html/index.html

## EC2 USER DATA

* It is possible to bootstrap our instances using an EC2 USER DATA script.
* BOOTSTRAPPING means launching commands when a machine starts.
* That script is only run once at the instance first start.
* EC2 user data is used to automate boot task such as:
1. Installing updates
2. Installing software
3. Downloading common files from the internet
4. We want to make sure that this EC2 instance has an Apache HTTP server installed on it-to display simple web page

## EC2 SPOT INSTANCES AND SPOT FLEETS

1. Can get a discount of upto 90% compared to ON-Demand
2. Define max spot price and get the instance while current spot price < max
  * The  hourly spot price varies based on offer and capacity
  * If the current spot price > your max price you can choose to stop or terminate your instance with a 2 minutes grace period.
3. Other strategy : Spot Block
  * Block spot instance during a specified time frame (1 to 6 hours)
  * In rare situations,the instance may be reclaimed
4. Used for batch jobs data analysis or workloads that are resilient to failures.
5. Not great for critical jobs or Databases

SPOT FLEETS

1. Spot Fleets= set of Spot instances + (optional) On Demand instances
2. The spot fleet will try to meet the target capacity with price constraints
  * Define possible launch pools (different instances,OS,AZ)
  * The fleet can choose most appropriate launch pool from multiple.
  * When spot fleet reaches the budget it will stop launching instances.
3. Strategies to allocate spot instance
   * Lowest price 
   * Diversified: distributed across all the pools
   * Capacity Optimized:
4. Spot fleets allow us automatically request spot instances with lowest price  

## EC2 INSTANCE TYPES-MAIN ONES

* R:application that needs a lot of RAM -in memory cache 
* C:applications that needs good CPU-compute /databases
* M:applications that are balanced-general /web app
* I:applications that need good local I/O(instance storage)-databases
* G:applications that need a GPU-video rendering /machine learning

* T2/T3:burstable instance
* T2/T3 -unlimited:unlimited burst

### BURSTABLE INSTANCES (T2/T3)

* AWS has the concept of burstable instances (
  T2/T3 machines)
* When you process something and there is something unexpected spike of loading your application and CPU Utilization reaches 100%.In this case CPU can burst and burst like a boost of power and cpu is very good during the burst.
* If your machines burst it utilizes "burst credits".If Burst credits are all gone and then CPU becomes bad
* When your load is over CPU stop bursting and gain back credits overtime and accumalate credits
* Burstable instances can be amazing to handle unexpected traffic.

T2/T3 UNLIMITED
* You pay extra money if you go over credit balance,but you don't lose in performance.Its new offering and cost is high if you are not monitoring the health of your instances. 

## CUSTOM AMI(Advantages)
* AMI is region locked and the same ID cannot be used across regions
* Pre-installed packages needed
* Faster boot time 
* Machines comes configured with monitoring /enterprise software
* Security Concers-We have control over the machines in the network 
* Contol of maintenance and update of AMIs over time
* Installing your app ahead of time
* Using someone else's AMI that is optimised for running an app,DB etc

AMI ARE BUILT FOR A SPECIFIC REGION

## PUBLIC AMI

* You can use public AMI from other people and can also rent other AMI from other people if they want to sell it.
* AMI can be found and published on the Amazon Marketplace

### AMI STORAGE

* Your AMI live in Amazon S3.Amazon S3 is a durable,cheap and resilient storage where most of your backups will live 
* By default your AMIs are private and locked for your account/region
* You can make AMIs public and share them with other AWS accoiunts or sell them on AMI marketplace
* it live in Amazon S3 and get charged for actual space in Amazon S3

AMAZON S3 PRICING IN US-EAST
* First 50TB /Month : $0.023 per GB
* Next 450 TB /Month:$0.022 per GB

### FAQ (AMI)

* You can share AMI with another AWS account
* If you share the AMI the ownership of the AMI does not changes.
* If someone copy your AMI to another region they become the owner
* You need someone to not copy your AMI you either not grant EBS snapshot access or S3 bucket access

LIMITS
* You can't copy an AMI with an associated billing product code that was shared with you from another account.This include windows AMIs and AMIs from AWS Marketplace have there billing product
* To copy shared AMIs with Billing product code,launch an EC2 instance in your account using shared AMI and then create an AMI with the Instance.
* You cant copy an encrypted AMI that was shared with you from another account.

## PLACEMENT GROUPS

* Placement groups are used when we want to have control over how EC2 instance are placed within AWS Infrastructure.

STRATEGY
1. Cluster-Gives you high performance computing and networking .Instances will be grouped together in low-latency hardware setup in single AZ
2. Spread-Instances are spread across diff hardware
3. Partition-Instances are spread across diff partitions.
![SOLUTIONS_ARCHITECT](placemnt_group.png)

## ELASTIC NETWORK INTERFACE(ENI)

* Logical component in a VPC that represents a virtual network card
* An ENI consists of primary IPv4 ,one or more secondary IPv4
* One Elastic IP(ipv4) per private IPv4
* one or more security groups
* You can create ENI independently from your EC2 instances and attach them on fly.
* Bound to a specific availability zone (AZ)

## EC2 HIBERNATE

* All the in-memory state is preserved that means all the data in your RAM is going to be preserved
* That means when you restart your instances after hibernating it,the instance boot is going much faster because operating system has not be stopped or restarted
* Upon we stop and hibernate RAM is going to be Dumped onto the encrypted Amazon EBS root volume and hence instance  will be stopped but not the OS. 
* Instance RAM size must less than 150GB
* Available for ON-Demand and Reserved Instances
* An instance cannot be hibernated more than 60 days.

## EC2 FOR SOLUTIONS ARCHITECT

* EC2 instances are billed by the second, t2.micro is free tier
* SSH on port 22 means you have to Lock down the security group to your IP
* Timeout issues-Due to security group issues
* permission issue after SSH use the command chmod 0400
* Security groups can have IP as rule and they can also reference other security groups instead of IP ranges and allowing dynamic set of EC2 instances access other EC2 instances
* We can customize an EC2 instance using a boot strap script also calles EC2 user data

DEDICATED HOSTS
Vendor licence bills based on the physical cores and underlying network socket visibility.

# HIGH AVAILABILITY AND SCALABILITY :ELB & ASG

## LOAD BALANCER-Good to know

* Load balancer can scale but if you have massive scale to do contact AWS for warm up.
* ERRORS-4xx errors are client induced errors,5xx errors application induced errors,Load Balancer errors 503 means no registered target
* MONITORING-ELB access log will log all access requests.Cloudwatch Metrics will gave you aggregate statistics

### ALB(application load balancer(v2))

* Application load balancer is layer 7(HTTP)
* Load balancing to multiple HTTP applications across machines (target groups)
* Load balancing to multiple applications on the same EC2 instance(ex;containers)
* Support for HTTP/2 and websocket
* Support redirects (from HTTP to HTTPS for EG:)
* It supports Routing
  - Routing based on path in URL
  - Routing based on hostname in URL
  - Routing based on Query string and Headers
  - ALB are great fit for micro services and container based application
* It has a port mapping feature to redirect to a dynamic port in ECS  

### NETWORK LOAD BALANCER (v2)

* Network load balancer(Layer4) allows to:
 - Forward TCP & UDP traffic to your instances
 - Allows to Handle millions of request per seconds
 - Latency is less for NLB ~100ms whereas ALB have 400ms
 - NLB is very different because they have one static IP per AZ(helpful for whitelisting specific IP)
 - NLB are used for extreme performance,TCP or UDP traffic
 - Not included in AwS free tier

 ### LOAD BALANCER STICKINESS

 * Its possible to implement stickiness so that the same client is always redirected to the same instnaces that is behind load balancer.It works for CLB & ALB 
 * Cookie has an expiration date as long as cookie remains same then the load balancer will redirect to the right instances 
 * Enabling stickiness can bring a imbalance to the load over EC2 instance in the backend because load is not evenly distributed  
 EG;client A wants to talk to the load balancer,which will redirect to the Ec2 instance on the left side,and if the same client talks again to the load balancer
 with stickiness enabled then the same Ec2 instance will receive the same results.For client B talk to the same load balancer but it will not direct to first EC2 instance but two second Ec2 instance, client C also be redirected to the second Ec2 instance as well.

 ### LOAD BALANCING CROSS-ZONE

 * With cross Zone load balancing each load balanceing instance will distribute traffic across instances in all AZ.In ALB it will always enabled and free of cost but in NLB it will cost if its enabled.

 ### ELB -SSL/TLS Certificates

 * SSL refers to SECURE SOCKET LAYER is used for encrypt transfer connections
 * SSL certificate allows traffic between your clients and your Load balancer to be encrypted in trasit and called In-flight encryption
 * It means data is going through a network and is going to be encrypted and is able to decrypted by the sender and receiver.

 * TLS is newer version of SSL and refers TRANSPORT LAYER SECURITY(TLS)
 * The Load balancer uses an X.509 certificate
 * You can manage certificate using ACM(AWS Certificate Manager) 

 HTTPS Listener you must specify
  1. Default certificate
  2. Clients can use SNI(SERVER NAME INDICATION) to specify the hostname they reach.

  ### SNI(Server Name Indication)

  * SNI solves the problem of loading multiple SSL certificates onto one web sever
  * Its newer protocol and requires client to indicate hostname of target server in SSL certificate.
   EG:So the client will says to ALB I want to connect to this website and ALB will says i have seen your request let me use correct SSL certificate.Client connects to an ALB having two target groups Domaim1.example.com and www.mycorp.com and has two SSl certificates corresponds to above tg

### ELB -CONNECTON DRAINING  

* For CLB its---connection draining                      
* For Target Group its---- DEREGISTRATION DELAY
* Its time to complete in-Flight requests while instance is de-registering or unhealthy
* The idea is that ELB will allow instance to shutdown before being De-registered and instance is in draining mode it will stop sending new requests to the instance and ELB may send requests to other instances connected to it.
* Between 1 to 3600 seconds,default 300 second ,to disable set 0.

## ASG (auto scaling group)

* You have Scaling policies for your ASG it can be anything such as CPU,Network or even custom metrics
* ASGs use launch configuration or launch templates
* To update an ASG you must provide new launch configuration/launch template
* IAM roles attached to ASG will get assigned to EC2 instances.
* ASG are free
* If you have an instances under an ASG,the beautiful thing is that if somehow the instances get terminated, then the ASG will realize that your instance has been terminated,and will automatically create new Instance as a replacement.
* ASG can terminate instances marked as unhealthy by an LB

SCALING COOLDOWNS
* its cooldown which allows your ASGto kind of settle before a new scaling. 
* This periods helps to ensure that your auto scaling group does't launch or terminate additional instance before previous scaling activity takes effect.
* other than default cooldown we also can create cooldown that apply for specific scaling policy.
* The common use of scaling specific cooldown is is that have a policy that terminates instances based on specific criteria or metric
* If your scaling up and down multiple times each hour,modify auto scaling groups cooldown timer and cloudwatch alarm period that triggers scale-in

## FOR EXAMS

* ASG default termination policy
1. Find the AZ which has more number of instances.
2. if there are multiple instances in same AZ it will delete one with oldest launch conf.
* Lifecycle hooks
1. When an instance is launced by ASG at first it go into pending state.we can define a lifecyclehook in pending state consists of:
    * Pending Wait
    * Pending Proceed
when terminating LIFECYCLEHOOK consists
  * Terminating wait
  * Terminating proceed

LAUNCH CONFIGURATION(legacy)
* Must be re-created every time

LAUNCH TEMPLATE(newer)
* Can have mutiple versions
* provisions both On-Demand and Spot instances
* Can use T2 unlimited burst feature
* recommended by AWS going forward

# EC2 STORAGE-EBS & EFS

## EBS VOLUME TYPES
1.  GP2(SSD): General purpose SSD volume that balances price and performance wide variety workloads.Can use it as system boot volumes,virtual desktop,low latency interactive apps.
1GB - 16TB
3 IOPS /GB min-100 IOPS,max-16000 IOPS,burst-3000IOPS

2. IO1 (SSD): Highest-performance SSD low latency
* Large database workloads(Mongodb,cassandra,microsoft SQL server,MySQL,PostgeSQL,Oracle)
4GB-16 TB
* min-100 ,max-64000
* for each gigabyte you can request 50 IOPS

3. ST1 (HDD):Low cost HDD volume designed for frequently accessed workloads.used in Big data,data warehouse,log processing
* 500 GB-16 TB
* Max IOPS -500

4. SC1 (HDD):Lowest cost HDD volume designed for less frequent workloads.cannot be a boot volume
* 500 GB-16 TB
* Max IOPS -250


EBS volumes are characterized in size
* When you create instances you can choose GP2 and IOI can be used as boot volumes

Making an Amazon EBS volume available for use on Linux(Browse in amazon documentation)
* lsblk
* sudo file -s /dev/xvdb (no file system in device and need to create )
* sudo mkfs -t ext4 /dev/xvdb
* sudo mkdir /data( to create a folder naming data)
* sudo mount /dev/xvdb /data ---(xvdb drive has been mounted to data folder) lsblk ----cd /data
* sudo touch hello.txt
* sudo nano hello.txt
* sudo cp /etc/fstab /etc/fstab.orig
* sudo nano /etc/fstab(enter /dev/xvdb /data ext4 defaults,nofail 0 2)
* to check its mounted sudo file -s /dev/xvdb
* to unmount sudo umount /data

## EBS SNAPSHOTS

* Its going to backup only  changed blocks
* EBS backup will use IO and you should't run while application is handling a lot of traffic
* Snapsots will be stored in S3
* Not necessary to detach volume to do snapshot but recommended
* MAx 100,000 snapshots per account
* can copy snapshot across AZ or Regions
* Can make Image(AMI) from snapshot
* EBS volumes that are restored by a snapshot need to pre-warmed(using fio or dd command)

## EBS ENCRYPTION

* When you create an encrypted EBS volume you get
  1. Data at rest is encrypted inside the volume
  2. All the date moving between instance and EBS volume is encrypted
  3. All snapshots are encrypted
  4. All volumes created from snapshot are encrypted

## EC2 INSTANCE STORE

* Physical disk attached to the physical server where your EC2 is
* Very High IOPS
* Disks upto 7.5 TB
* Cannot be increased in size
* risk of data loss if hardware fails

## EBS RAID OPTIONS

* Its used when you need to increase IOPS to 100,000 IOPS
* Its used to mirror EBS volumes.

RAID 0 (increase performance)
1. combining 2 or more volumes and getting the total space
2. using this we can built very big disk with a lot of IOPS
EG:we have 2 500 GB amazon EBS IO1 volume with 4000 provision IOPS will create 1000 Gb RAID 0 array with a available bandwith of 8000 IOPS and 1000 MB of throughput

RAID 1 (Increase fault performance)
* Mirroring volume together
* used when we need to send data to two EBS volume at same time.

## ELASTIC FILE SYSTEM-EFS

* Its used in content management ,web serving,data sharing,wordpress
* uses NFSv4.1 protocol
* To access EFS file system you need to use security group.
* Encryption at rest using KMS
* Its going to use only in POSIX file system(~Linux) that has stnadard file API
* Its built to 1000s of concurrent NFS clients so thousands of EC2 instances mounting on NFS drive

PERFORMANCE -General purpose and max I/O
STORAGE TIERS-Standard and Infrequent access(EFS-IA)

### HANDS ON 
* Create an EFS file system
* create a security group naming efs-demo an attach it two in three AZ in in EFS
* create 2 EC2 instances in 2 AZ and ssh into both
* sudo yum install -y amazon-efs-utils in both ec2 ssh terminal and go into demo efs and click on attach and we will find commands
* mkdir efs in both
*sudo mount -t efs -o tls fs-f83f5729:/ efs in both ssh user
edit security group efs-demo and gave inbound rules as NFS and custom as ec2-efs security group
* cd efs/ and sudo touch hello-world.txt and ll and we will see the the file is created and sudo nano hello-world.txt(type a string)
* we can see that everything created on one ec2 user can be automatically shared to other because of function of SHARED NETWORK FILE SYSTEM

# SOLUTIONS ARCHITECTURE DISCUSSION

* 