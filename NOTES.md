# Strategy

Use the `serverless-http` adapter library to wrap the existing Express application into an AWS Lambda handler.

Architecture:

```txt
Client
   ↓
Lambda Function URL
   ↓
AWS Lambda
   ↓
serverless-http adapter
   ↓
Express Application
```

This approach allows the existing Express application to run on AWS Lambda without rewriting the application logic.

---

# Reason for Choosing This Strategy

This strategy was selected because it requires minimal source code changes while keeping the existing Express application structure intact.

Benefits:

- Reuse existing routes and middleware
- Keep business logic unchanged
- Avoid rewriting the application
- Faster implementation and deployment
- Easier debugging and maintenance
- Separate local runtime and cloud runtime cleanly

The project structure remains clean:

```txt
app.js      -> business logic
server.js   -> local development server
lambda.js   -> AWS Lambda entrypoint
```

---

# Cold Start Measurement

Cold start performance was measured using Amazon CloudWatch logs.

Location:

```txt
Lambda
→ Monitor
→ View CloudWatch Logs
```

Measured values:

```txt
Init Duration: 432.15 ms
Duration: 24.91 ms
```

Results:

| Type | Measured Time |
|---|---|
| Cold Start | ~432 ms |
| Warm Request | ~25 ms |

Observation:

- Cold start performance is acceptable for a lightweight Node.js + Express application.
- Warm requests are significantly faster because the Lambda execution environment is reused.