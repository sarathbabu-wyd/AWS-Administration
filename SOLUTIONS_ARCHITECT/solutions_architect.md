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