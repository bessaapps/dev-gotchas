# Deploying a Next.js App with AWS Elastic Beanstalk and GitLab

## IAM

Set up an Elastic Beanstalk role with a trust relationship with Elastic Beanstalk and add the following permission policies:

- AWSElasticBeanstalkService</li>
- AWSElasticBeanstalkEnhancedHealth</li>

Set up an EC2 role with a trust relationship EC2

## Elastic Beanstalk

Create application then create environment with sample code. You must include the following across the various steps:

- Environment Name
- Platform (Node.js)
- Your Elastic Beanstalk role, and your EC2 role

Only use t3.small or higher. Anything less will time out while
installing packages.

- Add application environment (.env) variables.

## CodePipeline

- Choose the default S3 location.
- Connect GitLab, choose repository, and choose branch.
- Choose and set up CodeBuild. (Latest version of Ubuntu)
- Add application environment (.env) variables.
- Choose your Elastic Beanstalk application and environment for
deployment.

## Storage

You will need settings similar to the following to set up your buckets.

## CORS Policy

```json
[{
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "HEAD", "PUT"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": [],
    "MaxAgeSeconds": 3000,
}]
```

## Permissions

```json
[{
    "Version": "2012-10-17",
    "Id": "Policy123456789",
    "Statement": [
      {
        "Sid": "Statement1",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::<Bucket Name>/*"
      }
  ]
}]
```