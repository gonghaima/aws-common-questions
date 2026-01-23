How do you secure APIs in API Gateway against unauthorized access and common web vulnerabilities?

---

Amazon API Gateway provides several mechanisms to secure APIs:

**1. Authentication & Authorization:**

- Use AWS IAM roles and policies to control access for AWS users and services.
- Integrate with Amazon Cognito for user authentication and authorization.
- Implement custom Lambda authorizers for flexible access control logic.
- Require API keys for usage tracking and access control.

**2. Resource Policies:**

- Restrict access to APIs based on IP addresses, VPCs, or AWS accounts using resource policies.

**3. Encryption:**

- Enforce HTTPS (SSL/TLS) for secure data transmission.
- Support for request/response encryption.

**4. Protection Against Web Vulnerabilities:**

- Enable AWS WAF (Web Application Firewall) to protect against common threats like SQL injection, XSS, and DDoS attacks.
- Validate and sanitize input data using Lambda functions or backend services.

**5. Throttling & Quotas:**

- Set rate limits and quotas to prevent abuse and protect backend services from excessive traffic.

**6. Monitoring & Logging:**

- Use Amazon CloudWatch for logging, monitoring, and alerting on suspicious activity or errors.

These features help ensure APIs are protected against unauthorized access and common web vulnerabilities, providing a secure foundation for microservices architectures.
