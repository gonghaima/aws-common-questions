## How do you configure service discovery and load balancing for ECS services using an Application Load Balancer (ALB)?

**Short answer**
Attach the ECS service to an ALB target group, map container ports, configure health checks, and (optionally) add service discovery via AWS Cloud Map for internal DNS-based discovery.

**Step-by-step (ECS with ALB)**
1. **Create an ALB**
   - Pick public vs internal based on exposure.
   - Place it in at least two subnets across AZs.

2. **Create a target group**
   - Target type: `ip` for Fargate, `instance` for EC2.
   - Protocol/port should match the container listener (often HTTP/HTTPS).
   - Configure health check path (e.g., `/health`).

3. **Create ALB listeners and rules**
   - Listener on `80` or `443` (TLS with ACM certificate).
   - Forward to the target group.
   - Add path-based rules if routing multiple services (e.g., `/api/*`, `/web/*`).

4. **Define the ECS task and container ports**
   - In task definition, set `containerPort` and `protocol`.
   - For awsvpc networking (Fargate), you usually don't set `hostPort` (or set it equal to `containerPort`).

5. **Create/update the ECS service**
   - Attach the service to the ALB target group.
   - Choose the correct container name/port mapping.
   - Enable health check grace period if the app needs warm-up.

6. **Networking and security**
   - ALB SG allows inbound from the internet or VPC and outbound to tasks.
   - Task SG allows inbound from ALB SG on container port.
   - Ensure subnets and route tables are correct for public vs internal access.

7. **Auto scaling (recommended)**
   - Use target-tracking on CPU/memory or ALB request count per target.

**Service discovery (optional but common for internal services)**
- **Use AWS Cloud Map** when services need to discover each other internally via DNS.
- In the ECS service, enable service discovery and register in a Cloud Map namespace.
- Clients resolve `service.namespace` to get task IPs; use ALB for external traffic and Cloud Map for internal calls.

**Common interview callouts**
- For Fargate, target group **must** be `ip`.
- Health checks drive target registration/deregistration and deployment stability.
- Path-based routing lets one ALB serve multiple ECS services.
- Blue/green or canary deployments can be done via CodeDeploy + ALB listener rules.
