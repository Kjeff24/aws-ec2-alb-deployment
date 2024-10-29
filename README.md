# CloudFormation Template for Web Application Deployment

## Overview

This CloudFormation template deploys a web application on EC2 instances located in a private subnet with an Application Load Balancer (ALB) in public subnets across two availability zones. It sets up a highly available architecture that utilizes auto-scaling groups to ensure the application can handle varying levels of traffic.

## Template Details

- **AWSTemplateFormatVersion:** '2010-09-09'
- **Description:** Deploy a web application on EC2 instances with an ALB in public subnets across two availability zones.

## Parameters

- **VpcCIDR**: (String) The CIDR block for the VPC (default: `10.0.0.0/16`).
- **PublicSubnet1CIDR**: (String) CIDR for the first public subnet (default: `10.0.1.0/24`).
- **PublicSubnet2CIDR**: (String) CIDR for the second public subnet (default: `10.0.2.0/24`).
- **PrivateSubnet1CIDR**: (String) CIDR for the first private subnet (default: `10.0.3.0/24`).
- **PrivateSubnet2CIDR**: (String) CIDR for the second private subnet (default: `10.0.4.0/24`).
- **InstanceType**: (String) EC2 instance type (default: `t2.micro`).
- **KeyName**: (AWS::EC2::KeyPair::KeyName) Name of an existing EC2 KeyPair for SSH access.
- **ImageId**: (String) AMI ID for the EC2 instances (default: `ami-06b21ccaeff8cd686`, Amazon Linux 2 AMI ID for us-east-1).

## Resources

- **VPC and Subnets**: The template creates a VPC with two public and two private subnets.
- **Internet Gateway**: An Internet Gateway is created and attached to the VPC for public subnets.
- **Route Tables**: Public route tables are configured to allow traffic from the internet.
- **Security Groups**: Two security groups are created:
    - **ALB Security Group**: Allows incoming traffic on port 80.
    - **EC2 Instance Security Group**: Allows incoming traffic from the ALB on port 80 and SSH access on port 22.
- **Load Balancer**: An Application Load Balancer is created in public subnets to distribute incoming traffic to EC2 instances.
- **Target Group**: Defines the instances that the Load Balancer will route traffic to.
- **Launch Template**: Defines the configuration for launching EC2 instances, including the user data script that sets up a simple web page.
- **Auto Scaling Group**: Configures auto-scaling for EC2 instances across two private subnets.

## Outputs

- **LoadBalancerDNSName**: The DNS name of the Load Balancer, which can be used to access the web application.

## Usage

1. Ensure that you have an existing EC2 KeyPair for SSH access.
2. Adjust the parameters as needed in the CloudFormation console or CLI.
3. Create a new stack using this template in the AWS CloudFormation service.
4. Once the stack creation is complete, use the `LoadBalancerDNSName` output to access your web application.

## Notes

- The EC2 instances are configured to run a Python HTTP server, serving the HTML content stored in the /home/ec2-user/html directory.
  The User Data script in the launch template sets up this HTTP server and binds it to port 80.
- The default instance type is `t2.micro`, which is suitable for testing and small workloads.
- Ensure that the necessary IAM permissions are in place for creating these resources.
