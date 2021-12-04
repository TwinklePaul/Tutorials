# CREATING A PROPER CI/CD PIPELINE:

## 1. Create an AWS Account.

## 2. Create a New IAM User with admin privileges.
This user should be always used in future. Root user account must be avoided at all possible costs.

## 3. From the security credentials, create IAM Access Keys for the root user. 
Download these access keys and securely store them.

## 4. Download AWS CLI.

## 5. Configuring AWS CLI.

## 6. Download Docker.
### 6.1. Establishing New Connections
* For general use, the aws configure command is the fastest way to set up your AWS CLI installation. When you enter this command, the AWS CLI prompts you for four pieces of information:
- Access key ID
- Secret access key
- AWS Region
- Output format

```
    aws configure
    aws configure list
```

* The AWS CLI stores this information in a profile (a collection of settings) named default in the credentials file. 

* By default, the information in this profile is used when you run an AWS CLI command that doesn't explicitly specify a profile to use.

### 6.2. Using existing configuration and credentials files
```
    aws configure import --csv file://credentials.csv
```
* To use the config and credentials files, move them to the folder named .aws in your home directory.

* You can specify a non-default location for the config and credentials files by setting the AWS_CONFIG_FILE and AWS_SHARED_CREDENTIALS_FILE environment variables to another local path.

#### How to set environment variables?
The following examples show how you can configure environment variables for the default user.
```
    export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
    export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    export AWS_DEFAULT_REGION=us-west-2
```

### 6.3. Setting AWS Profiles
```
    aws configure --profile produser
    aws configure list-profiles
    aws s3 ls --profile produser
```
* A collection of settings is called a profile. By default, the AWS CLI uses the default profile. 

* You can create and use additional named profiles with varying credentials and settings by specifying the `--profile` option aand assigning a name.

* You can then specify a `--profile` profilename and use the credentials and settings stored under that name.
```
    aws s3 ls --profile produser
```

*To update these settings, run `aws configure` again (with or without the `--profile` parameter, depending on which profile you want to update) and enter new values as appropriate.*

### 6.3. Set and view configuration settings
```
    aws configure set region us-west-2 --profile integ
    aws configure get region --profile integ
```
* You can set any credentials or configuration settings using aws configure set. Specify the profile that you want to view or modify with the --profile setting.

* You can retrieve any credentials or configuration settings you've set using aws configure get. Specify the profile that you want to view or modify with the --profile setting.



## 7. Run the official AWS CLI version 2 Docker image
The official AWS CLI version 2 Docker image is hosted on DockerHub in the amazon/aws-cli repository. The first time you use the docker run command, the latest Docker image is downloaded to your computer. Each subsequent use of the docker run command runs from your local copy.

To run the AWS CLI version 2 Docker image, use the docker run command.
```
    docker run --rm -it amazon/aws-cli <command>

    Example:
    docker run --rm -it amazon/aws-cli --version
```

This is how the command functions:
* Each time you run this command, Docker spins up a container of your downloaded amazon/aws-cli image, and executes your aws command. 

* By default, the Docker image uses the latest version of the AWS CLI version 2.

* `--rm` – Specifies to clean up the container after the command exits.

* `-it` – Specifies to open a pseudo-TTY with stdin. 
    * This enables you to provide input to the AWS CLI version 2 while it's running in a container, 
    * For example, by using the `aws configure` and `aws help` commands. 
    * If you are running scripts, -it is not needed. 
    * If you are experiencing errors with your scripts, omit -it from your Docker call.

* The only tool supported on the image is the AWS CLI. Only the aws executable should ever be directly run. 

* The /aws working directory is user controlled. The image will not write to this directory, unless instructed by the user in running an AWS CLI command.

* You can also choose to use the latest AWS CLI (default) version or a specific version as shown below:
```
    docker run --rm -it amazon/aws-cli:latest command
    or
    docker run --rm -it amazon/aws-cli:2.0.6 command
```

* To manually update to the latest version, we recommend you pull the latest tagged image. Pulling the Docker image downloads the latest version to your computer.
```
    docker pull amazon/aws-cli:latest
```

## 8. Share host files, credentials, environment variables, and configuration
Because the AWS CLI version 2 is run in a container, by default the CLI can't access the host file system, which includes configuration and credentials.

To share the host file system, credentials, and configuration to the container:
* mount the host system’s `~/.aws` directory to the container at `/root/.aws` with the `-v` flag to the `docker run` command. 
* This allows the AWS CLI version 2 running in the container to locate host file information.
```
    docker run --rm -it -v ~/.aws:/root/.aws amazon/aws-cli command
```

### Some Examples:

#### 1. Shorten the Docker command
* For basic access to aws commands, run the following:
```
    alias aws='docker run --rm -it amazon/aws-cli'
```

* For access to the host file system and configuration settings when using aws commands, run the following.:
```
    alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'
```

#### 2. Providing credentials and configuration

```
    docker run --rm -it -v ~/.aws:/root/.aws amazon/aws-cli s3 ls
```
* We're providing host credentials and configuration when running the s3 ls command to list your buckets in Amazon Simple Storage Service (Amazon S3). 

#### 3. Using your AWS_PROFILE environment variable

```
    docker run --rm -it -v ~/.aws:/root/.aws -e AWS_PROFILE amazon/aws-cli s3 ls
```
* You can call specific system's environment variables using the -e flag. Call each environment variable you'd like to use.
* We're providing host credentials, configuration, and the AWS_PROFILE environment variable when running the s3 ls command to list your buckets in Amazon Simple Storage Service (Amazon S3).

#### 4. Downloading an Amazon S3 file to your host system

```
    docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli s3 cp s3://aws-cli-docker-demo/hello .
```
* For some AWS CLI version 2 commands, you can read files from the host system in the container or write files from the container to the host system.

* In this example, we download the S3 object `s3://aws-cli-docker-demo/hello` to your local file system by mounting the current working directory to the container's `/aws` directory. 

* By downloading the `hello object` to the container's `/aws` directory, the file is saved to the host system’s current working directory also.

* To confirm the downloaded file exists in the local file system, run the following.
```
    cat hello
```

