I'll create a comprehensive README.md file that captures the key information from the infrastructure documentation.



# Book Viewer AWS Infrastructure

## Overview

This repository contains a CloudFormation template for deploying a scalable AWS infrastructure for the Book Viewer application in the `eu-central-1` region. The infrastructure provides a robust, production-ready setup with multiple AWS services.

## Architecture Components

- **VPC**: Custom VPC with two public subnets
- **RDS**: Multi-AZ PostgreSQL database
- **ECS**: Fargate-based cluster with blue/green deployment
- **ALB**: Internet-facing load balancer
- **S3**: Storage bucket for book uploads
- **CodeDeploy**: Zero-downtime deployment configuration

## Prerequisites

- AWS account with necessary permissions
- AWS CLI configured with credentials
- Docker image pushed to ECR
- S3 bucket `book-viewer-v2` created in `eu-central-1`

## Deployment Instructions

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Deploy CloudFormation Stack

```bash
aws cloudformation create-stack \
  --stack-name book-viewer-stack \
  --template-body file://template.yaml \
  --parameters \
      ParameterKey=DBName,ParameterValue=myappdb \
      ParameterKey=DBUsername,ParameterValue=myappuser \
      ParameterKey=DBPassword,ParameterValue=<your-secure-password> \
  --region eu-central-1 \
  --capabilities CAPABILITY_IAM
```

### 3. Monitor Deployment

```bash
aws cloudformation describe-stacks --stack-name book-viewer-stack --region eu-central-1
```

## Key Outputs

- **RDS Endpoint**: Database connection endpoint
- **ALB DNS**: Load balancer URL for accessing the application
- **CodeDeploy Application**: Deployment configuration details

## Network Configuration

- **VPC CIDR**: `10.0.0.0/16`
- **Subnets**:
    - `10.0.1.0/24` (eu-central-1a)
    - `10.0.2.0/24` (eu-central-1b)

## Security Considerations

### Security Groups
- **ALB**: Allows HTTP, HTTPS, ports 8080, 8081
- **ECS**: Allows port 8080 from ALB, port 5432 to RDS
- **RDS**: Allows port 5432 (publicly accessible)

### IAM Roles
- Task Execution Role
- S3 Access Role
- CodeDeploy Service Role
- ECS Auto Scaling Role

## Usage

### Accessing the Application
- Use the BookViewerLoadBalancerURL
- Database connections via SSM parameters

### Updating the Application
- Utilize CodeDeploy for blue/green deployments

## Cleanup

To delete the entire stack:

```bash
aws cloudformation delete-stack --stack-name book-viewer-stack --region eu-central-1
```

## Important Notes

- RDS is publicly accessible—consider restricting in production
- ECR is in `eu-west-1`, other resources in `eu-central-1`
- Use a secure password during deployment

## Contributing

- Fork the repository
- Submit issues or pull requests
- Contribute to infrastructure improvements
