# When would you choose Lambda over containers (ECS/EKS) for a backend service, and when is Lambda a poor fit?

## Choose Lambda When:

### Event-Driven & Sporadic Workloads
- **API endpoints with unpredictable traffic** - Lambda scales to zero when idle
- **File processing triggers** - S3 uploads, DynamoDB changes
- **Scheduled tasks** - Cron jobs, batch processing
- **Webhook handlers** - Third-party integrations

### Cost Optimization
- **Low to medium traffic** - Pay only for execution time
- **Intermittent usage patterns** - No idle server costs
- **Short-running tasks** - Under 15 minutes execution time

### Operational Simplicity
- **No server management** - AWS handles infrastructure
- **Automatic scaling** - Concurrent execution scaling
- **Built-in monitoring** - CloudWatch integration
- **Rapid deployment** - Fast iteration cycles

## Lambda is a Poor Fit When:

### Performance Requirements
- **Cold start sensitivity** - Sub-100ms response requirements
- **Long-running processes** - Over 15 minutes execution
- **High memory needs** - Over 10GB RAM
- **Persistent connections** - WebSockets, database pools

### Consistent High Traffic
- **Predictable steady load** - Containers more cost-effective
- **Always-on services** - Continuous processing needs
- **High throughput** - Thousands of concurrent requests

### Complex Dependencies
- **Large deployment packages** - Over 250MB unzipped
- **Custom runtime environments** - Specific OS configurations
- **Stateful applications** - Session management, caching
- **Multi-service coordination** - Complex microservice orchestration

## Decision Framework

**Choose Lambda for:**
- Event-driven microservices
- API backends with variable traffic
- Data processing pipelines
- Serverless-first architectures

**Choose Containers (ECS/EKS) for:**
- Always-on web applications
- Complex multi-container applications
- Custom runtime requirements
- Predictable, sustained workloads

## Cost Comparison Example

**Lambda:** $0.20 per 1M requests + $0.0000166667 per GB-second
**ECS/EKS:** EC2 instance costs + container orchestration overhead

*Lambda wins for sporadic traffic; containers win for consistent load*
