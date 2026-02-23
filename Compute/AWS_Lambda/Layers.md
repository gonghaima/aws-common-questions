# Lambda Layers

**What are Lambda Layers, and how can they help you share common Node.js code (utilities, SDK configuration) across multiple functions?**

## What are Lambda Layers?

Lambda Layers are a distribution mechanism for libraries, custom runtimes, and other function dependencies. They allow you to package code that can be shared across multiple Lambda functions, reducing deployment package size and promoting code reuse.

## Key Benefits

- **Code Reuse**: Share common utilities, libraries, and configurations
- **Smaller Deployment Packages**: Reduce function size by extracting dependencies
- **Faster Deployments**: Update shared code once instead of in every function
- **Version Management**: Layer versioning for dependency control
- **Runtime Optimization**: Faster cold starts with pre-loaded dependencies

## Common Use Cases for Node.js

### 1. Shared Utilities Layer
```javascript
// Layer: /opt/nodejs/utils/logger.js
module.exports = {
  info: (message) => console.log(`[INFO] ${new Date().toISOString()}: ${message}`),
  error: (message) => console.error(`[ERROR] ${new Date().toISOString()}: ${message}`)
};

// Layer: /opt/nodejs/utils/validator.js
module.exports = {
  validateEmail: (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email),
  validatePhone: (phone) => /^\+?[1-9]\d{1,14}$/.test(phone)
};
```

### 2. AWS SDK Configuration Layer
```javascript
// Layer: /opt/nodejs/aws-config.js
const AWS = require('aws-sdk');

AWS.config.update({
  region: process.env.AWS_REGION || 'us-east-1',
  httpOptions: {
    timeout: 30000,
    connectTimeout: 5000
  }
});

module.exports = {
  dynamodb: new AWS.DynamoDB.DocumentClient(),
  s3: new AWS.S3(),
  sns: new AWS.SNS()
};
```

### 3. Using Layers in Lambda Functions
```javascript
// Lambda function using the layers
const { info, error } = require('/opt/nodejs/utils/logger');
const { validateEmail } = require('/opt/nodejs/utils/validator');
const { dynamodb } = require('/opt/nodejs/aws-config');

exports.handler = async (event) => {
  try {
    info('Processing user registration');
    
    const { email, name } = JSON.parse(event.body);
    
    if (!validateEmail(email)) {
      return { statusCode: 400, body: 'Invalid email' };
    }
    
    await dynamodb.put({
      TableName: 'Users',
      Item: { email, name, createdAt: Date.now() }
    }).promise();
    
    return { statusCode: 200, body: 'User created' };
  } catch (err) {
    error(`Registration failed: ${err.message}`);
    return { statusCode: 500, body: 'Internal error' };
  }
};
```

## Layer Structure for Node.js

```
layer.zip
└── nodejs/
    ├── node_modules/     # npm dependencies
    ├── utils/           # custom utilities
    │   ├── logger.js
    │   └── validator.js
    └── aws-config.js    # AWS SDK configuration
```

## Creating and Deploying Layers

### Using AWS CLI
```bash
# Create layer package
zip -r layer.zip nodejs/

# Publish layer
aws lambda publish-layer-version \
  --layer-name shared-utilities \
  --zip-file fileb://layer.zip \
  --compatible-runtimes nodejs18.x nodejs20.x

# Add layer to function
aws lambda update-function-configuration \
  --function-name my-function \
  --layers arn:aws:lambda:region:account:layer:shared-utilities:1
```

### Using Serverless Framework
```yaml
# serverless.yml
service: my-service

layers:
  sharedUtils:
    path: layers/shared-utils
    compatibleRuntimes:
      - nodejs18.x
      - nodejs20.x

functions:
  userHandler:
    handler: handlers/user.handler
    layers:
      - { Ref: SharedUtilsLambdaLayer }
```

## Best Practices

- **Layer Size**: Keep layers under 50MB (unzipped)
- **Versioning**: Use semantic versioning for layer updates
- **Dependencies**: Include only necessary dependencies
- **Organization**: Group related functionality in single layers
- **Testing**: Test layer compatibility across Node.js versions
- **Security**: Regularly update dependencies for security patches

## Interview Key Points

1. **Reduces cold start time** by pre-loading common dependencies
2. **Promotes DRY principle** across Lambda functions
3. **Simplifies deployment** by separating business logic from utilities
4. **Enables team collaboration** through shared layer libraries
5. **Supports up to 5 layers per function** with 250MB total limit