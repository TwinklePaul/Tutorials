# Configuring Terraform

## 1. Define provider and version.

## 2. Define variables in .tfvars file.
Store the aws secret arns here with the specific name
This is a file where you can store the variables.

## 3. Create the terraform backend.
If you have an MFA setup, make sure that the profile that you are using also has the mfa_serial setup in ~/.aws/config
```
    [profile admin]
    region = us-east-1
    output = json
    mfa_serial = arn:aws:iam:<Serial Number>:mfa/<UserName>
```

To know more about configuring the code pipeline with terraform, goto the following links:
[Youtube Tutorial](https://www.youtube.com/watch?v=JwTP3wZHYnU 'Youtube Tutorial')
[Github Link](https://github.com/davoclock/aws-cicd-pipeline 'Github Link')