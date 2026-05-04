# Security Best Practices

The following are recommended security hardening measures to consider when deploying this solution in production.

1. **Amazon Simple Notification Service (Amazon SNS) Topic AWS Key Management Service (AWS KMS) Encryption** — Enable server-side encryption with a customer-managed KMS CMK on the SNS topic. This solution uses it for low-sensitivity notification data (AMI IDs, PR links).

2. **AWS Secrets Manager CMK Encryption** — Use a customer-managed KMS key instead of the default AWS-managed key (`aws/secretsmanager`) for stricter key rotation and access control.

3. **AWS Lambda Environment Variable Encryption** — Encrypt AWS Lambda environment variables with a customer-managed KMS key. This solution stores only non-sensitive config (ARNs, repo names, branch names) — secrets are fetched at runtime from Secrets Manager.

4. **Specific Exception Handling** — The release notes fetcher uses a broad `except Exception` for resilience. In production, narrow to specific exception types (`URLError`, `HTTPError`, `JSONDecodeError`).

5. **Lambda Input Validation** — Add schema validation on Lambda event payloads if functions are exposed to untrusted callers. In this solution, all invocations come from trusted AWS services (Amazon EventBridge, AWS Step Functions).

6. **Amazon Virtual Private Cloud (Amazon VPC) Deployment for Lambda** — Deploy Lambda functions into a VPC with private subnets if they access VPC-bound resources. This solution accesses only AWS APIs and GitHub over HTTPS, so VPC deployment is optional.

7. **Lambda Dead Letter Queues** — Configure Amazon Simple Queue Service (Amazon SQS) dead letter queues on Lambda functions for asynchronous invocations. In this solution, Step Functions handles retries and error states natively.

8. **Amazon CloudWatch Logs KMS Encryption** — Create explicit Amazon CloudWatch Log Groups with KMS encryption and a retention policy. This solution's logs contain only operational data (AMI IDs, analysis summaries), no secrets or PII.

## Reporting a Vulnerability

If you discover a potential security issue in this project, we ask that you notify AWS Security via our [vulnerability reporting page](https://aws.amazon.com/security/vulnerability-reporting/). Please do **not** create a public GitHub issue.
