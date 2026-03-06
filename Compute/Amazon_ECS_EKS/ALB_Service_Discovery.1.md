## How do you configure service discovery and load balancing for ECS services using an Application Load Balancer (ALB)?

To configure service discovery and load balancing for ECS services with an Application Load Balancer (ALB):

1. **Create an ALB:**
   - In the AWS Console, create an Application Load Balancer and specify subnets and security groups.
   - Define listeners (typically HTTP/HTTPS) and target groups.

2. **Configure ECS Service:**
   - When creating an ECS service (Fargate or EC2 launch type), select the ALB as the load balancer.
   - Choose the target group for your service. ECS will automatically register/deregister tasks in the target group.
   - Set the health check path and protocol for the target group.

3. **Enable Service Discovery (Optional):**
   - Use AWS Cloud Map for service discovery. When creating the ECS service, enable service discovery and specify a namespace.
   - ECS will automatically register tasks with Cloud Map, allowing other services to discover them via DNS or API.

4. **Task Definition:**
   - Ensure your task definition exposes the correct ports and protocols.
   - Use dynamic port mapping if running multiple tasks per host.

5. **IAM Permissions:**
   - ECS tasks and services need permissions to register with the load balancer and service discovery.

6. **Testing:**
   - Deploy the service and verify that the ALB routes traffic to healthy ECS tasks.
   - Use DNS names from Cloud Map for service-to-service communication if service discovery is enabled.

**Summary:**
ECS integrates tightly with ALB for load balancing, automatically managing target group registration. Service discovery via AWS Cloud Map allows dynamic discovery of task endpoints, supporting microservices architectures. This setup is common for scalable, resilient containerized applications in AWS.
