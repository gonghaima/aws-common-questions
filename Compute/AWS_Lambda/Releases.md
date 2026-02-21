How would you implement function versioning and aliases to safely roll out new Lambda releases (for example, blue/green or canary)?

## Answer

Lambda versioning and aliases enable safe deployment strategies:

### Versioning
- Each time you publish a function, AWS creates a **version** with a unique ARN (e.g., `arn:aws:lambda:region:account:function:name:1`)
- Versions are immutable - once published, they cannot be changed
- `$LATEST` always points to the latest unpublished code

### Aliases
- Aliases are mutable pointers to a specific version (e.g., `PROD`, `STAGING`)
- Allow routing traffic between versions via **weighted routing**

### Blue/Green Deployment
1. Publish new version (e.g., version 5)
2. Update alias to point to new version:
   ```bash
   aws lambda update-alias --function-name myFunc --name PROD --function-version 5
   ```
3. Rollback by pointing alias back to previous version

### Canary Deployment
Use weighted routing to gradually shift traffic:
```bash
aws lambda put-function-event-invoke-config \
  --function-name myFunc \
  --alias-name PROD \
  --destination-config '{"OnFailure":{"Destination":"arn:aws:lambda:region:account:function:error-handler"}}'
```

Then update routing:
```bash
aws lambda update-alias --function-name myFunc --name PROD \
  --routing-config '{"AdditionalVersions":{"FunctionVersion":"5","Weight":0.1}}'
```

This sends 10% of traffic to v5, 90% to current version. Gradually increase weight to 1.0 for full rollout.

### Best Practices
- Use **environment variables** for configuration differences between versions
- Enable **provisioned concurrency** for latency-sensitive canary releases
- Set up **Amazon CloudWatch alarms** to auto-rollback on errors