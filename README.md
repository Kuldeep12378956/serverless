# S3 replication using serverless. 


## Below are the steps to perform S3 replication using serverless. 

![serverless-s3-replication](https://github.com/Kuldeep12378956/serverless/assets/154441092/960b17d5-3384-46e7-ab70-caaa86d5097d)

1- Create Destination bucket using cloud formation. The Cloudformation template is persent in ops repository under path cf/s3/


2- Create SSM Parameter in AWS system Manager to add ARN of destination bucket created by cloud formation in specified environment.


3- Deploy the serverless.

Note :- The destination bucket must have to created in specified env before running serverless.


4- Run the sync command in the cloudshell. to sync the existing data in Source bucket to the destinatiob bucket

```
aws s3 sync <source-bucket> <destination-bucket>
```
