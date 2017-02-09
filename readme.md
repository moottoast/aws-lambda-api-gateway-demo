# AWS Lambda and API Gateway Intro

## Lambda
- Runtime: Node.js
- Blueprint: hello-world
- No trigger
- Name
- Explain parts of code
  - `'use strict'` - application compatibility and better syntax checking
  - `console.log` - standard logging; will show up in Cloudwatch logs and tests
  - `exports` - the standard Node object
  - `handler` - the function Lambda invokes when executing
  - `event` - parameter to pass in event data (like any variables)
  - `context` - runtime data about the Lambda function iteself
  - `callback` - data returned upon completion of the function (error, success)
- New code:
```javascript
'use strict';

console.log('Loading function');

exports.handler = (event, context, callback) => {
    let min = 0;
    let max = 10;

    let generatedNumber = Math.floor(Math.random() * max) + min;

    callback(null, generatedNumber);
};
```

- (Scroll down)
- Handler - index.handler
- Role - Create a new role from template
- Role name
- Policy Template - Simple Microservices permissions
- Memory (runs in container; also related to processing power)
- Timeout (5 minute max; kills and return error message if times out)
- Create
- Test

## API Gateway
- Create API
- Name
- Action - New Resource
- Resource name = number
- Action - New Method - GET
- Integration type = Lambda Function
- Region = us-east-1
- Function = random-number-generator
- Show Request/Response cycle
- Actions - Deploy API - create new stage
- Stages - PROD - /number - GET - invoke URL
- Visit URL
- Resources - GET - Integration Request
- Body Mapping Templates - Add mapping template - `application/json`
- Code for template:
```json
{
    "min": $input.params('min'),
    "max": $input.params('max')
}
```

- Save
- Redeploy API
- Back to Lambda function
- In Code - `event` parameter now will hold values for 'min' and 'max'
```javascript
'use strict';

console.log('Loading function');

exports.handler = (event, context, callback) => {
    let min = event.min;
    let max = event.max;

    let generatedNumber = Math.floor(Math.random() * max) + min;

    callback(null, generatedNumber);
};
```

- Visit URL and add parameters: `...?min=1&max=100`
