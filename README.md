# Sports API Management System

## Project Overview

A containerized API management system for real-time sports data, leveraging AWS cloud services for scalable, serverless architecture with robust API management and container orchestration.

## Key Features

- Real-time sports data REST API
- Serverless containerized backend using Amazon ECS Fargate
- Secure API management with Amazon API Gateway
- Scalable cloud infrastructure

## Technical Stack

- **Cloud Provider**: AWS
- **Compute**: Amazon ECS (Fargate)
- **API Management**: Amazon API Gateway
- **Monitoring**: CloudWatch
- **Language**: Python 3.x
- **Containerization**: Docker

## Prerequisites

- Sports API key from serpapi.com
- AWS account
- AWS CLI configured
- Docker installed
- Basic knowledge of:
  - ECS
  - API Gateway
  - Docker
  - Python

## Technical Architecture

![Image](https://lucid.app/publicSegments/view/bf95e2f6-b2d5-49b1-a991-2a3925cf3ec8/image.png)

## Project Structure

```bash
sports-api-management/
├── app.py           # Flask application
├── Dockerfile       # Container definition
├── requirements.txt # Python dependencies
├── .gitignore
└── README.md        # Project documentation
```

## Deployment Guide

### 1. Repository Setup

```bash
git clone https://github.com/ifeanyiro9/containerized-sports-api.git
cd containerized-sports-api
```

### 2. ECR Repository Creation

```bash
aws ecr create-repository --repository-name sports-api --region us-east-1
```

### 3. Docker Image Build and Push

```bash
# Authenticate with AWS ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

# Build and tag image
docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:latest
```

### 4. ECS Cluster Configuration

#### Cluster Creation

- Create Fargate cluster named `sports-api-cluster`

#### Task Definition

- Task Definition: `sports-api-task`
- Infrastructure: Fargate
- Container Configuration:
  - Image: ECR repository image
  - Port: 8080 (TCP)
  - Environment Variable: `SPORTS_API_KEY`

#### Service Deployment

- Capacity Provider: Fargate
- Desired Tasks: 2
- Load Balancer: Application Load Balancer
- Health Check Path: `/sports`

### 5. API Gateway Setup

1. Create REST API
2. Add `/sports` resource
3. Configure HTTP Proxy integration
4. Deploy to production stage

### 6. Verification

Test API endpoint:

```bash
curl https://<api-gateway-id>.execute-api.us-east-1.amazonaws.com/prod/sports
```

## Security Considerations

- Least privilege IAM policies
- Secure API Gateway endpoints
- Environment-based configuration management

## Future Roadmap

- Implement Amazon ElastiCache for request caching
- Add DynamoDB for user query tracking
- Enhanced API authentication
- CI/CD pipeline for automated deployments

## Troubleshooting

- Verify AWS CLI configuration
- Check ECR repository permissions
- Validate Docker image build
- Review ECS task definition settings
- Monitor CloudWatch logs for deployment issues

## Estimated Costs

- ECR: $0.10 per GB/month
- ECS Fargate: $0.04048 per vCPU per hour
- API Gateway: $3.50 per million API calls
- Recommended monthly budget: $50-$100 depending on traffic

## Contributing

1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create pull request
