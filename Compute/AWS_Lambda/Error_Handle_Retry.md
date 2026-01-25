How do you design error handling and retries in Lambda when consuming from SQS or Kinesis (dead‑letter queues, partial failures, visibility timeout)?

**Answer:**

When designing error handling and retries for AWS Lambda functions triggered by SQS or Kinesis, consider the following best practices:

---

### 1. **Dead-Letter Queues (DLQ):**
- **SQS:**
  - Configure a DLQ (another SQS queue) for the source queue. Messages that fail processing after the maximum receive count are automatically moved to the DLQ for later analysis or reprocessing.
- **Kinesis:**
  - Kinesis does not natively support DLQs. Implement a custom DLQ by sending failed records to an SQS queue or another storage (e.g., DynamoDB, S3) within your Lambda error handler.

### 2. **Retries and Partial Failures:**
- **SQS:**
  - Lambda automatically retries batch processing until the message is successfully processed or the visibility timeout expires. If a batch contains multiple messages and one fails, the entire batch is retried.
  - Use the `reportBatchItemFailures` feature (for SQS batch event source) to allow Lambda to only retry failed messages instead of the whole batch (supported in recent Lambda runtimes).
- **Kinesis:**
  - Lambda retries the entire batch until the data expires (default 7 days). If a single record fails, the whole batch is retried.
  - Use error handling logic to skip or isolate problematic records, and consider checkpointing or storing failed records elsewhere.

### 3. **Visibility Timeout (SQS):**
- Set the SQS visibility timeout longer than the Lambda function timeout to prevent other consumers from processing the same message while Lambda is still working on it.
- Example: If Lambda timeout is 5 minutes, set visibility timeout to at least 6 minutes.

### 4. **Monitoring and Alerts:**
- Use Amazon CloudWatch metrics and alarms to monitor Lambda errors, DLQ message counts, and processing latency.
- Set up alerts for high error rates or DLQ activity to respond quickly to issues.

### 5. **Best Practices:**
- Keep Lambda functions idempotent to handle retries safely.
- Log failed events with enough context for debugging.
- For Kinesis, consider using enhanced fan-out and parallelization to reduce the impact of a single bad record.

---

**Summary Table:**
| Feature                | SQS Lambda Trigger                | Kinesis Lambda Trigger           |
|------------------------|-----------------------------------|----------------------------------|
| DLQ Support            | Native (SQS DLQ)                  | Custom (SQS/S3/DynamoDB)         |
| Retry Behavior         | Retries batch/failed messages      | Retries entire batch             |
| Partial Failure        | `reportBatchItemFailures` support  | Not natively supported           |
| Visibility Timeout     | Configurable (should > Lambda)     | Not applicable                   |
| Message Retention      | Up to 14 days                      | Up to 7 days                     |

---

**References:**
- [AWS Lambda with SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
- [AWS Lambda with Kinesis](https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html)
- [Error handling in Lambda](https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventsourcemapping.html#event-source-mapping-error-handling)