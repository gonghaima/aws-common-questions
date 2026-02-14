How would you monitor and troubleshoot a Lambda microservice using CloudWatch Logs, metrics, and X‑Ray tracing?

## CloudWatch Logs

**Structured Logging**
```python
import json
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

def lambda_handler(event, context):
    # Structured logging with correlation ID
    correlation_id = event.get('requestId', context.aws_request_id)
    
    logger.info(json.dumps({
        'correlationId': correlation_id,
        'event': 'function_start',
        'userId': event.get('userId')
    }))
    
    try:
        result = process_request(event)
        logger.info(json.dumps({
            'correlationId': correlation_id,
            'event': 'function_success',
            'duration': context.get_remaining_time_in_millis()
        }))
        return result
    except Exception as e:
        logger.error(json.dumps({
            'correlationId': correlation_id,
            'event': 'function_error',
            'error': str(e),
            'errorType': type(e).__name__
        }))
        raise
```

**Log Insights Queries**
```sql
-- Find errors in last hour
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 100

-- Track function duration trends
fields @timestamp, @duration
| filter @type = "REPORT"
| stats avg(@duration) by bin(5m)
```

## CloudWatch Metrics

**Key Metrics to Monitor**
- **Duration** - Function execution time
- **Invocations** - Number of function calls
- **Errors** - Failed executions
- **Throttles** - Concurrent execution limits hit
- **DeadLetterErrors** - Failed async retries

**Custom Metrics**
```python
import boto3

cloudwatch = boto3.client('cloudwatch')

def put_custom_metric(metric_name, value, unit='Count'):
    cloudwatch.put_metric_data(
        Namespace='MyApp/Lambda',
        MetricData=[
            {
                'MetricName': metric_name,
                'Value': value,
                'Unit': unit,
                'Dimensions': [
                    {
                        'Name': 'FunctionName',
                        'Value': context.function_name
                    }
                ]
            }
        ]
    )

# Usage
put_custom_metric('ProcessedRecords', len(records))
put_custom_metric('BusinessLogicLatency', processing_time, 'Milliseconds')
```

**CloudWatch Alarms**
```yaml
# CloudFormation template
ErrorRateAlarm:
  Type: AWS::CloudWatch::Alarm
  Properties:
    AlarmName: Lambda-HighErrorRate
    MetricName: Errors
    Namespace: AWS/Lambda
    Statistic: Sum
    Period: 300
    EvaluationPeriods: 2
    Threshold: 5
    ComparisonOperator: GreaterThanThreshold
    Dimensions:
      - Name: FunctionName
        Value: !Ref MyLambdaFunction
```

## X-Ray Tracing

**Enable Tracing**
```python
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.core import patch_all

# Patch AWS SDK calls
patch_all()

@xray_recorder.capture('lambda_handler')
def lambda_handler(event, context):
    # Add metadata
    xray_recorder.put_metadata('user_id', event.get('userId'))
    xray_recorder.put_annotation('environment', 'prod')
    
    return process_request(event)

@xray_recorder.capture('database_call')
def query_database(user_id):
    # This will show as separate subsegment
    return dynamodb.get_item(Key={'userId': user_id})
```

**Custom Subsegments**
```python
@xray_recorder.capture('external_api_call')
def call_external_service(data):
    subsegment = xray_recorder.current_subsegment()
    subsegment.put_annotation('api_endpoint', 'payment-service')
    
    try:
        response = requests.post('https://api.example.com', json=data)
        subsegment.put_metadata('response_status', response.status_code)
        return response.json()
    except Exception as e:
        subsegment.add_exception(e)
        raise
```

## Troubleshooting Strategies

**1. Performance Issues**
```python
# Cold start optimization
import time

# Global variables for connection reuse
db_connection = None

def lambda_handler(event, context):
    global db_connection
    start_time = time.time()
    
    # Initialize connection once
    if db_connection is None:
        db_connection = create_db_connection()
        logger.info(f"Cold start - connection initialized in {time.time() - start_time:.2f}s")
    
    return process_with_connection(db_connection, event)
```

**2. Memory and Timeout Monitoring**
```python
def lambda_handler(event, context):
    import psutil
    
    # Log memory usage
    memory_usage = psutil.virtual_memory().percent
    remaining_time = context.get_remaining_time_in_millis()
    
    logger.info(json.dumps({
        'memoryUsage': memory_usage,
        'remainingTime': remaining_time,
        'allocatedMemory': context.memory_limit_in_mb
    }))
    
    if remaining_time < 5000:  # Less than 5 seconds
        logger.warning('Function approaching timeout')
```

**3. Error Correlation**
```python
def lambda_handler(event, context):
    correlation_id = event.get('correlationId', context.aws_request_id)
    
    # Add to all log entries
    logger = logging.getLogger()
    logger.addFilter(lambda record: setattr(record, 'correlationId', correlation_id) or True)
    
    # Add to X-Ray
    xray_recorder.put_annotation('correlationId', correlation_id)
```

## Monitoring Dashboard

**Key Widgets to Include**
- Function invocation count and error rate
- Average duration and P99 latency
- Concurrent executions vs limits
- X-Ray service map showing dependencies
- Log error patterns and frequency

**Automated Alerting**
- Error rate > 1% for 5 minutes
- Duration > P95 baseline for 10 minutes
- Throttling events detected
- Dead letter queue messages accumulating

This comprehensive monitoring approach provides visibility into Lambda performance, errors, and dependencies, enabling quick identification and resolution of issues in microservice architectures.
