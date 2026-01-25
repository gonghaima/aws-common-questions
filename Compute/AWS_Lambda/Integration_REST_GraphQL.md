Explain how you would integrate a Lambda‑based Node.js backend with API Gateway to build a REST or GraphQL API.

To integrate a Lambda-based Node.js backend with API Gateway for building a REST or GraphQL API, follow these steps:

1. **Develop the Lambda Function (Node.js):**

   - Write your backend logic in a Node.js Lambda function. For REST, structure your code to handle different HTTP methods and paths. For GraphQL, use a library like `apollo-server-lambda` to handle GraphQL queries and mutations.

2. **Create an API Gateway:**

   - For REST APIs, use API Gateway REST API. For GraphQL, you can use either REST API (with a single POST endpoint) or API Gateway HTTP API for lower latency.

3. **Configure Routes/Resources:**

   - For REST: Define resources and methods (GET, POST, etc.) in API Gateway, mapping each to the appropriate Lambda function.
   - For GraphQL: Typically, create a single POST endpoint (e.g., `/graphql`) that forwards all requests to your Lambda.

4. **Set Up Integration:**

   - In API Gateway, set the integration type to Lambda and select your Node.js Lambda function.
   - Configure request/response mapping templates if needed (for REST, to map HTTP requests to Lambda event format).

5. **Deploy the API:**

   - Create and deploy a stage in API Gateway (e.g., `dev`, `prod`).
   - Obtain the invoke URL for client access.

6. **(Optional) Secure the API:**

   - Use API keys, IAM roles, or Cognito authorizers for authentication and authorization.

7. **Test the Integration:**
   - Use tools like Postman or curl to send requests to your API Gateway endpoint and verify Lambda execution.

**Example (REST):**

- API Gateway receives a GET request at `/users`.
- API Gateway triggers the Lambda function, passing event data.
- Lambda (Node.js) processes the request and returns a response.
- API Gateway formats and returns the response to the client.

**Example (GraphQL):**

- API Gateway receives a POST request at `/graphql`.
- API Gateway triggers the Lambda function running Apollo Server.
- Lambda parses and resolves the GraphQL query, returning the result.

**Best Practices:**

- Use environment variables for configuration.
- Implement proper error handling and logging in Lambda.
- Consider using AWS SAM or Serverless Framework for deployment automation.
- For production, enable caching, throttling, and monitoring in API Gateway.
