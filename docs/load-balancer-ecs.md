# Using a Load Balancer with ECS Fargate
This guide goes through the necessary steps to create a Load Balancer and to use it to expose REST APIs hosted on Fargate.

To expose an API on a LB you first need to create a Target Group (basically defines the target for a Load Balancer listener) and a Load Balancer with a Listener that points at that target group.

*In general*: create the TG and LB before creating the ECS service.<br>
This is needed because when creating the ECS service you willl have the possibility to specify which LB and TG to use and the ECS Service will automatically register the Tasks as Target Group's Targets.

### Important Note
> The problem with this approach is that you will need **one LB for each service on ECS**.<br> 
That happens because there is no routing in the LB, only balancing load. 

This is **highly expensive**, since you will pay the full cost of an ALB for **each service**, which would be around **20$ per month** (per service).

## 1. Create a Target Group
Under `EC2 > Target groups`, create a new Target Group.

> Target Groups that target ECS Fargate APIs should use the *Target Type*: **IP Addresses**. <br>
*This is needed because Fargate assigns an IP to each task running under a Service (instead of using EC2 instances)*. 

The Port of the TG should be the port exposed by the container running the API on ECS (e.g. 8080).

Select the **VPC** that the ECS cluster is attached to. 

You should skip the target registration. <br>
The reason being that ECS tasks will self register with the Target Group when creating the ECS Service.<br> 
*I suggest always starting by creating the Target Group before creating the ECS Service, otherwise you'll have to register the IP Addresses of the different tasks running under the Service as a target in the TG, which is very manual and error-prone.*

## 2. Create an Application Load Balancer
Under `EC2 > Load Balancers`, create a new **Application Load Balancer**.

The ALB in this case should be **Internet Facing**.<br>

When selecting Availability Zones, make sure you select only AZs where there's also the ECS Cluster running.<br>
**That does not mean that the LB needs to be in the same subnet, but only in the same AZs**.<br>
Note that **at least two subnets need to be specified** and those subnets need to be availble in **two different AZ**. 

Make sure you select a **Security Group** that allows for *inbound and outbound internet access* **on the port specified**. 
> *When creating Security Groups, I only had a SG that was open on 8080. I had to modify it to allow traffic on port 80 too.*

When creating a Listener, you can select **any port** (e.g. 80), since the TG will be the one that directs the traffic on 8080 on the containers.

## 3. Create a Service on ECS Fargate
If you have already created an ECS Service, delete it and create a new one. 

When creating the ECS Service, make sure that you do the following: 
* Select **ALB**
* Use an **existing LB**, and select the LB you created earlier.
* Use an **existing listener** and select the one that was created with the ALB. 
* Use an **existing Target Group** and select the one you created earlier. 

## 4. Testing that it works
Using the DNS name of the LB you should be able to call it on port 80 and it will route to the ECS Service.

## 5. Cleaning up resources
To clean up all the resources in this guide: 
1. Delete the Service.
2. Delete the Load Balancer.
3. Delete the Target Group.
4. Delete any additional Subnet that was created for this purpose.