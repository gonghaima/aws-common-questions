What is the difference between REST APIs, HTTP APIs, and WebSocket APIs in API Gateway?

---

Amazon API Gateway supports three main types of APIs, each designed for different use cases:

**1. REST APIs:**
- Full-featured, highly configurable APIs for RESTful web services.
- Support for request/response transformation, custom authorizers, API keys, usage plans, and more.
- Suitable for complex API requirements and legacy integrations.
- Higher cost and latency compared to HTTP APIs.

**2. HTTP APIs:**
- Lightweight, low-latency APIs for building modern web and mobile backends.
- Simpler configuration and lower cost than REST APIs.
- Support for OAuth 2.0/JWT authorizers, CORS, and direct integration with AWS Lambda and other services.
- Lacks some advanced features of REST APIs (e.g., request/response mapping templates).
- Recommended for most new HTTP-based workloads.

**3. WebSocket APIs:**
- Enable real-time, two-way communication between clients and servers over WebSocket protocol.
- Ideal for chat apps, live dashboards, and real-time notifications.
- Supports connection management, message routing, and integration with AWS Lambda.

**Summary:**
- Use **REST APIs** for advanced features and legacy support.
- Use **HTTP APIs** for most new, simple, and cost-effective HTTP workloads.
- Use **WebSocket APIs** for real-time, bidirectional communication needs.