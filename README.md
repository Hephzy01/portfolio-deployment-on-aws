# Portfolio Website Deployment on AWS using GitHub Actions CI/CD

## Project Overview

This project demonstrates the deployment of a static portfolio website on AWS using a fully automated CI/CD pipeline.

The website source code is maintained in a private GitHub repository while this public repository documents the architecture, deployment workflow, implementation process, and lessons learned during the project.

The objective was to build a production style serverless hosting platform that automatically deploys website updates with minimal operational overhead while providing HTTPS, global content delivery, and custom domain support.

---

## Architecture 


---

## Architecture Workflow

1. The developer pushes code changes to the private GitHub repository.

2. GitHub Actions automatically triggers the deployment workflow.

3. GitHub Actions authenticates with AWS using IAM credentials stored securely in GitHub Secrets.

4. The workflow synchronizes website files to an S3 bucket configured for static website hosting.

5. CloudFront retrieves website assets from the S3 origin and distributes them through edge locations globally.

6. AWS Certificate Manager provides an SSL certificate for HTTPS communication.

7. The domain purchased from a third party registrar points to Route 53 name servers.

8. Route 53 routes traffic to the CloudFront distribution.

9. Users access the website securely over HTTPS.

---

## Services Used

| Service                     | Purpose                                     |
| --------------------------- | ------------------------------------------- |
| GitHub Repository           | Stores application source code privately    |
| GitHub Actions              | Automates deployment pipeline               |
| AWS IAM                     | Provides secure access for CI/CD deployment |
| Amazon S3                   | Hosts static website files                  |
| Amazon CloudFront           | Provides CDN and HTTPS delivery             |
| AWS Certificate Manager     | Manages SSL certificates                    |
| Amazon Route 53             | DNS hosting and traffic routing             |
| Third-party Domain Provider | Domain registration                         |

---

## CI/CD Workflow

The deployment pipeline performs the following actions automatically whenever code is pushed to the main branch:

* Trigger GitHub Actions workflow
* Authenticate to AWS using IAM credentials
* Upload updated files to S3
* Remove deleted files from the bucket
* Invalidate CloudFront cache
* Publish the latest version globally

This removes the need for manual uploads and reduces deployment time significantly.

---

## Security Implementation

The following security controls were implemented:

* Source code repository remains private.
* AWS credentials are stored using GitHub Secrets.
* IAM access is limited to deployment operations.
* HTTPS encryption is enforced using ACM certificates.
* DNS traffic is routed through CloudFront.

---

## Challenge Encountered

Secure Custom Domain Integration with HTTPS

**Issue**

Integrating a third-party domain with CloudFront while maintaining HTTPS required coordination across multiple AWS services.

**Cause**

The solution required proper configuration between:

AWS Certificate Manager (ACM)
CloudFront
Route 53 hosted zone
Third-party domain registrar DNS records

A misconfiguration in any of these components would prevent successful certificate validation or website accessibility.



Resolution

Requested SSL certificates through ACM.
Validated domain ownership using DNS validation records.
Configured Route 53 hosted zone records.
Updated the third-party registrar nameservers to point to Route 53.
Attached the validated certificate to the CloudFront distribution.

Lesson Learned

Custom domain configuration for globally distributed applications involves dependencies across DNS, certificate management, and CDN services that must be configured in the correct sequence.

---

## Outcome

The final solution provides:

* Fully automated deployments
* Serverless website hosting
* Global content delivery
* HTTPS support
* Custom domain integration
* Infrastructure cost optimisation
* Minimal operational overhead

---

## Future Improvements

Possible future enhancements include:

* Infrastructure as Code using Terraform
* GitHub OIDC authentication instead of long-lived IAM credentials
* AWS WAF integration
* Monitoring with CloudWatch
* Automated testing before deployment

---

## Author
Adeola Akinlade
