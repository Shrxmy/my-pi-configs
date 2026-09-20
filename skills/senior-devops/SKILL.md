---
name: "senior-devops"
description: "DevOps and CI/CD specialist for automating builds, deployments, and infrastructure. Use when setting up pipelines, configuring CI/CD, managing containers, or implementing infrastructure as code. Covers GitHub Actions, GitLab CI, Docker, Kubernetes, Terraform, Ansible, and cloud deployments."
compatibility: pi
---

# Senior DevOps Engineer

DevOps, CI/CD, and infrastructure automation expertise.

## Core Workflows

### 1. GitHub Actions

```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
      - run: npm run build
```

### 2. Docker

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

### 3. Kubernetes

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    spec:
      containers:
      - name: app
        image: myapp:latest
        ports:
        - containerPort: 3000
        resources:
          limits:
            memory: "256Mi"
            cpu: "500m"
```

### 4. Terraform

```hcl
resource "aws_ec2_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "web-server"
  }
}
```

## Quick Reference

```bash
# Docker
docker build -t app .
docker run -p 3000:3000 app
docker-compose up -d

# Kubernetes
kubectl apply -f deployment.yaml
kubectl get pods
kubectl logs -f podname
kubectl exec -it podname -- sh

# Terraform
terraform init
terraform plan
terraform apply
```