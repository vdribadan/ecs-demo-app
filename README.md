# Amazon ECS proof-of-concept with demo app

This repo was made in effort to create proof of concept infrastructure on Amazon ECS and consists of the following:

 - Cloudformation Template - creates VPC with public and private subnets in two availability zones.
 - Cloudformation Template - creates an ECS cluster with highly availabale and scalable service
 - Dockerfile - nginx reverse proxy 
 - Dockerfile - small nodejs app, showing some server stats
  
 ## Prerequisities
 
 - [x] An AWS account
 - [x] Keypair for ssh to amazon instances
 - [ ] **Optional** Amazon ECR repos or any other public repos if you want to build container images on your own
 


## Important note !!!
Please, note that ECS CloudFormation template is meant to be used in Stockholm (aws-north-1) region. If you want to run the setup in any other region - please, add respective AMIs to the templates




## VPC CloudFormation Template

A VPC with two subnets - private and public in two availability zones
![A VPC with two subnets - private and public in two availability zones](https://templates.cloudonaut.io/en/stable/img/vpc-2azs.png)

**Installation instructions:**

 1. Click [this](https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/stable/vpc/vpc-2azs.yaml&stackName=vpc) link or import template cloned from git repository on CloudFormation Stacks creation page
 2. Choose a name and all required parameters for the Stack
 3. Tick "I acknowledge..." and hit Create Stack 


## Demo App on ECS Template

This template creates an ECS cluster running on two EC2 nodes in different availability zones. With 2 sets of containers per each zone respectively and a Loadblancer, which distributes the load between frontend containers.
Service itself consists of the frontend reverse proxy (nginx) and backend (nodeJS) containers. See Dockerfiles. Also, you can look [here](http://ecsalb-444481108.eu-north-1.elb.amazonaws.com/) for the working example of the app.


![ECS cluster architetcture](https://templates.cloudonaut.io/en/stable/img/ecs-service-cluster-alb.png)
**Installation instructions:**

 1. Click [this](https://eu-north-1.console.aws.amazon.com/cloudformation/home?region=eu-north-1#/stacks/quickcreate?templateURL=https://s3.eu-north-1.amazonaws.com/cf-templates-2xcudetae43s-eu-north-1/2021049Imb-ecs-cluster-templatehai9j0i0va) link or import template cloned from current git repository on CloudFormation Stacks creation page
 2. Choose a name for the stack
 3. Choose your EC2 Keypair from the dropdown list
 4. Choose two Subnets. **Public A and B are recommended**, since you'll need to setup NAT instance if you want to run containers in the private subnets. ~~In To-Do list~~
 5. Select VPC, that was created by previous template
 6. Tick "I acknowledge..." and hit Create Stack
 7. Check the Outputs from the Stack creation and follow the Lodabalancer DNS name link to check if the app is running
 8. Go to Services > ECS and observe your ECS cluster for running instances of the app 




 

