Amazon ECS – core concepts
Explain what Amazon ECS is and how it orchestrates Docker containers for microservices.
​

Compare running ECS tasks on EC2 vs using Fargate; when would you choose each option?
​

What are the main building blocks in ECS (cluster, task definition, service, task, container) and how do they relate?
​

How do you configure service discovery and load balancing for ECS services using an Application Load Balancer (ALB)?
​

How would you deploy a Node.js microservice in ECS and handle zero‑downtime deployments?
​

Amazon ECS – scaling, resilience, and ops
How does ECS handle service‑level autoscaling, and which metrics would you typically scale on (CPU, memory, request count)?
​

How do you implement health checks for ECS services, and what happens when a container repeatedly fails them?
​

Describe how you would use CloudWatch, logs (for example, FireLens), and X‑Ray to monitor and debug ECS‑based microservices.
​

How would you design blue/green or canary deployments for ECS services (for example, using CodeDeploy or separate target groups)?
​

What are some strategies for secrets and configuration management in ECS (SSM Parameter Store, Secrets Manager, task role)?
​

Amazon EKS – Kubernetes on AWS
What is Amazon EKS, and how is it different from ECS in terms of control plane, abstraction level, and Kubernetes ecosystem support?
​

In what situations would you recommend EKS instead of ECS for a microservices platform?
​

Describe how Kubernetes primitives (Deployment, Service, Ingress, ConfigMap, Secret) map to a Node.js microservice running on EKS.
​

How would you expose an EKS workload to the internet using AWS integrations (for example, ALB Ingress Controller or NLB)?
​

What are some common add‑ons you would use with EKS for production (for example, Cluster Autoscaler, metrics server, Prometheus, Fluent Bit)?
​

EKS – scaling, networking, and cost
How does cluster autoscaling work in EKS, and how does it interact with pod‑level autoscaling (HPA)?
​

Explain how you would design network policies and security groups for EKS to isolate microservices and restrict traffic.
​

What are the trade‑offs between running EKS worker nodes on EC2 vs using Fargate profiles for certain workloads?
​

How would you approach cost optimization in an EKS cluster (right‑sizing nodes, bin packing, spot instances)?
​

How would you implement rolling updates and rollbacks for a Node.js deployment on EKS, and how do you control max unavailable / surge?
