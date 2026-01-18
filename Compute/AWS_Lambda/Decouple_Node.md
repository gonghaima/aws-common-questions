How would you structure a Node.js Lambda handler so that business logic is testable and not tightly coupled to the Lambda runtime?

---

To make business logic testable and decoupled from the Lambda runtime in Node.js:

**1. Separate Business Logic from Handler:**
- Place your core business logic in standalone modules or functions that do not depend on AWS Lambda-specific objects (event, context).
- The Lambda handler should only handle event parsing, input validation, and invocation of the business logic.

**2. Example Structure:**

```js
// businessLogic.js
function processOrder(order) {
  // ...business logic here...
  return { status: 'success', orderId: order.id };
}
module.exports = { processOrder };

// handler.js
const { processOrder } = require('./businessLogic');
exports.handler = async (event) => {
  const order = JSON.parse(event.body);
  const result = processOrder(order);
  return {
    statusCode: 200,
    body: JSON.stringify(result),
  };
};
```

**3. Benefits:**
- Business logic can be unit tested independently of the Lambda runtime.
- The handler remains thin and focused on integration with AWS Lambda.
- Easier to reuse and maintain business logic across different runtimes or services.

**Summary:**
Keep business logic pure and separate from Lambda-specific code to maximize testability and maintainability.