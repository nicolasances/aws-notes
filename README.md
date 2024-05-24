# AWS Notes
This repository contains notes on different AWS services and guides on how to implement common patterns on AWS. 

### Index

#### Pattern: Microservices on ECS Fargate
This pattern revolves around deploying microservices on ECS, using Fargate.

This pattern is based on the following architecture: 

![](./diagrams/Microservices%20on%20ECS.png)

1. Deploying Container Images on ECR
2. [Using a Load Balancer with ECS Fargate](docs/load-balancer-ecs.md)
3. [Configuring multiple ECS Services under a single ALB](docs/alb-multiple-ecs-services.md)
4. [Configuring your own domain with ALB and Route 53](docs/own-domain-name-alb.md)
5. [Configure secrets on AWS Secrets Manager](docs/managing-secrets.md)

#### Other

* [Configuring Boto3](docs/boto3.md)
* [Managing Secrets in AWS](docs/managing-secrets.md)