# CREATING A PROPER CI/CD PIPELINE:

## 1. Create an AWS Account.

## 2. Create a New IAM User with admin privileges.
This user should be always used in future. Root user account must be avoided at all possible costs.

## 3. From the security credentials, create IAM Access Keys for the root user. 
Download these access keys and securely store them.

## 4. Download AWS CLI.

## 5. Download Docker.

## 6. Run the official AWS CLI version 2 Docker image
The official AWS CLI version 2 Docker image is hosted on DockerHub in the amazon/aws-cli repository. The first time you use the docker run command, the latest Docker image is downloaded to your computer. Each subsequent use of the docker run command runs from your local copy.

To run the AWS CLI version 2 Docker image, use the docker run command.
```
    docker run --rm -it amazon/aws-cli command
```