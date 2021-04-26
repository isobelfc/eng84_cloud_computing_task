# Cloud Computing Task

- naming convention for anything we create is `eng84_name_description`

## Set Up VPC
- go to `Your VPCs` on AWS and click on `Create VPC`
- enter a name according to naming convention
- IPv4 CIDR block should be 2 numbers, then 2 zeros, then /16
- e.g. `12.12.0.0/16`

## Internet Gateway Setup
- this allows our VPC to connect to the internet
- name according to convention
- once created, select it from the list, and select `Attach to VPC`
- use the VPC created earlier

## Subnet Creation
- we will create 2 subnets, one public and one private
- name according to convention, and make sure to include whether public or private
- the IPv4 CIDR block must use the same first two numbers from our VPC, followed by a unique number, then a 0, and then /24
- e.g. `12.12.1.0/24`
- the IPv4 CIDR block for the second subnet must have a different third number
- e.g. `12.12.2.0/24`

## Route Tables
### Public Subnet Route Table
- on the Route Table page we can see a list of all route tables associated with VPCs created by our group
- looking through the list we can see which subnet each of them is associated with
- we can then name the associated Route Table to find it more easily
- under `Routes>Edit Routes` we can add a route that connects 0.0.0.0/0 to our internet gateway
- we can also associate our public subnet with the route table, under `Subnet Associations`

### Private Subnet Route Table
- we do not want our DB to have access to the internet
- to do this, we create a new route table, and attach it to our VPC
- we can then associate it with our private subnet, under `Subnet Associations`

## Creating Instances within Subnets
### App instance
- launch a new instance using Ubuntu 16.04
- use free t2 micro instance type
- set `Network` to your VPC
- choose your public subnet for `Subnet`
- enable auto-assignment of Public IP
- leave storage page on defaults
- add a name tag, following convention and including `app` within name
- create a new security group, allowing SSH access from `My IP` and HTTP access from anywhere
- after reviewing and clicking `Launch`, choose `DevOpsStudent` key pair

### DB instance
- follow the same general steps as above, except for:
- set subnet to your private one
- security group should allow SSH access from `My IP`, and `All traffic` access from the IPv4 of your public subnet

## Connect to instance
- navigate to `~/.ssh` folder on local machine
- run the ssh command given under `Connect to instance`
- copy the `app` folder from `dev_env` folder on local machine with `scp` command
- e.g. `scp -i ~/.ssh/DevOpsStudent.pem -r app/ ubuntu@app_ec2_public_ip:~/app/`
- run provision files with `./file_name.sh`

## NACL
- under `Network ACLs` in the VPC Dashboard, create a new NACL
- associate it with your VPC
- set inbound rules to allow HTTP and SSH from all sources, and All Traffic from the IPv4 of your VPC
- set outbound rules to allow All Traffic to any destination
- under `Subnet associations` associate both subnets