# AWS Authentication

The AWS related scan commands needs credentials in order to authenticate to AWS.
There are multiple ways to configure authentication and they are similar to how
`aws` cli authentication is setup.

### Credentials in environment

Set `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` variables in environment.
For example,

```bash
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

Note: If you are using temporary credentials, `AWS_SESSION_TOKEN` also needs to be set.

Refer: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html

### Named profile in credentials file

`vault-radar` can read credentials from the AWS Credentials file, usually located at `$HOME/.aws/config`.
By default, it looks for a profile with name `default`. However any other profile in the credentials file can be
chosen by setting `AWS_PROFILE` environment variable

Refer: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html#cli-configure-files-using-profiles

### EC2 Instance profile

When running `vault-radar` on an AWS EC2 instance, we can assign an IAM role for the EC2 instance and grant it
access to the resource that needs to be scanned.

These credentials will be available to the code running on the instance through the Amazon EC2 metadata service. 

Refer: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html


### ECS & EKS

It is also possible to run `vault-radar` as a container in AWS ECS (Elastic Container Service) or AWS EKS (Elastic Kubernetes Service).
We need to create an IAM role with a policy granting the access to the resource that needs to be scanned as assign it appropriately.
For ECS, we need to assign the IAM role to the task. 
For EKS, we need to create an IAM OIDC provider for the cluster and assign the role to a service account.

Refer: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html
Refer: https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html
