How do memory and timeout settings affect Lambda performance and cost, and how would you tune them for a high‑throughput API?

**Answer:**

**Memory Settings:**
Increasing Lambda memory allocation also increases CPU and network bandwidth proportionally. Higher memory can significantly reduce execution time for compute-intensive or I/O-bound workloads, improving throughput. However, it also increases cost per invocation, since AWS charges based on memory size and execution duration.

**Timeout Settings:**
Timeout determines the maximum allowed execution time for a Lambda function. Setting it too low may cause premature termination and errors, while setting it too high can lead to unnecessary cost if the function hangs or is inefficient. For high-throughput APIs, timeout should be just above the expected maximum execution time, with proper error handling and retries.

**Tuning for High-Throughput APIs:**
- **Benchmark and Profile:** Start with default settings, monitor execution time, and incrementally increase memory to find the optimal balance between speed and cost.
- **Optimize Code:** Ensure the function is efficient, minimizing cold starts and external calls.
- **Set Timeout Carefully:** Use metrics to determine realistic timeout values, avoiding excessive durations.
- **Monitor and Adjust:** Use AWS CloudWatch to track performance, errors, and costs, adjusting settings as needed.
- **Consider Parallelism:** For high-throughput, design the API to allow concurrent Lambda invocations, scaling horizontally.

**Summary:**
Tune memory to minimize execution time without overspending, and set timeout to prevent unnecessary costs or failures. Regularly monitor and adjust based on workload and performance metrics.

