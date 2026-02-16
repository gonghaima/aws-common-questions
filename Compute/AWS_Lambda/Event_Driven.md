AWS Lambda – architecture & patterns
Describe an event‑driven microservice built with S3, SQS, and Lambda that processes uploaded files end‑to‑end.

## Architecture Overview

```
User → S3 (Upload) → S3 Event → Lambda (Trigger) → SQS (Queue) → Lambda (Worker) → Processing → S3 (Output) / DynamoDB
```

## Step-by-Step Flow

1. **File Upload**: User uploads a file (e.g., CSV, JSON, image) to an S3 bucket.

2. **S3 Event Trigger**: S3 is configured to emit an event notification on object creation. This event is sent directly to an SQS queue (or SNS, Lambda directly).

3. **Lambda (Producer)**: A Lambda function is triggered by the S3 event. Its role is lightweight—validate the file metadata (size, type) and push a message to SQS for async processing. This decouples ingestion from processing.

4. **SQS Queue**: Acts as a buffer to handle burst traffic, prevent Lambda concurrency limits, and ensure reliability. Dead-letter queue (DLQ) handles failed messages.

5. **Lambda (Worker)**: Another Lambda polls the SQS queue, retrieves the file, and performs the actual processing (e.g., image resizing, CSV parsing, data transformation).

6. **Output**: Processed files are written back to a different S3 bucket, and metadata is stored in DynamoDB for tracking.

## Key Benefits

- **Decoupling**: SQS separates ingestion from processing, allowing independent scaling.
- **Reliability**: SQS ensures messages aren't lost; DLQ captures failures.
- **Cost efficiency**: Lambda scales automatically; you pay only for executed time.
- **Resilience**: If worker Lambda fails, SQS retries with exponential backoff.

## Considerations

- **Lambda concurrency limits**: Use SQS to buffer and smooth traffic spikes.
- **Idempotency**: Design worker Lambda to handle duplicate messages safely.
- **Timeout**: For long-running tasks, consider Step Functions or ECS/Fargate instead of Lambda.