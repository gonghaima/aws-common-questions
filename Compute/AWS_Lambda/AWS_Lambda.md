AWS Lambda – Node.js / backend specifics
How would you structure a Node.js Lambda handler so that business logic is testable and not tightly coupled to the Lambda runtime?
​

What are best practices for managing NPM dependencies and deployment package size for Lambda (for example, bundling, layers, devDependencies)?
​

How do you handle connection pooling from Lambda to databases like RDS or Aurora, and what problems can arise with many concurrent invocations?
​

Explain how you would integrate a Lambda‑based Node.js backend with API Gateway to build a REST or GraphQL API.
​

How do you design error handling and retries in Lambda when consuming from SQS or Kinesis (dead‑letter queues, partial failures, visibility timeout)?
​

AWS Lambda – performance, limits, and operations
What is a cold start in Lambda, and what factors impact cold start time for Node.js functions?
​

How can you mitigate cold starts (for example, provisioned concurrency, smaller bundles, avoiding heavy initialization)?
​
​

How do memory and timeout settings affect Lambda performance and cost, and how would you tune them for a high‑throughput API?
​

What are key service limits for Lambda (for example, max timeout, payload size, concurrent executions) and how do they influence design?
​

How would you monitor and troubleshoot a Lambda microservice using CloudWatch Logs, metrics, and X‑Ray tracing?
​

AWS Lambda – architecture & patterns
Describe an event‑driven microservice built with S3, SQS, and Lambda that processes uploaded files end‑to‑end.
​

When would you choose Lambda over containers (ECS/EKS) for a backend service, and when is Lambda a poor fit?
​

How would you implement function versioning and aliases to safely roll out new Lambda releases (for example, blue/green or canary)?
​

What are Lambda Layers, and how can they help you share common Node.js code (utilities, SDK configuration) across multiple functions?
​

How do you apply security best practices for Lambda in a multi‑service architecture (least‑privilege IAM, VPC access, secrets management)?
