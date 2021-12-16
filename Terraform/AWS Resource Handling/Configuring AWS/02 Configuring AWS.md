# CONFIGURING AWS

## 1. Create an S3 bucket.
Create a bucket to store the code for the pipeline. This bucket will be storing the same code that you'll be uploading to your github repo.

## 2. Store Docker Credentials as Secret Key.
* Goto the Secret manager of AWS.
* Create and Store a new Secret - store the dockerhub username and password here.
* Make sure to keep a copy of the arn.
* This credential will be used in your code to connect with docker.

## 3. Connect to Github.
* Goto Developer Tools -> CodePipeline -> Settings -> Connections.
* Create connection to your bitbucket or github account where you'll have your code pipeline.
* Store the id that gets generated as well as the arn.

## 4. Start working on your repository.
Perform all the usual steps associated with the creation of a new repo.


