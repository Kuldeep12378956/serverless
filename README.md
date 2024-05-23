# Step to Sync Existing Data of Source bucket into Destination Bucket in Production.

1. Create an EC2 instance in Production.
2. Create an IAM Role with s3 full access and attach it  with the Ec2 Instance created.
3. Install Docker in it.
4. Create a directory
5. In That directory Create a Dockerfile and a shell script file. with content.

# Dockerfile

```
# Use the official Ubuntu base image
FROM ubuntu:latest

# Install dependencies
RUN apt-get update && \
    apt-get install -y \
    python3 \
    python3-pip \
    curl \
    unzip

# Install AWS CLI v2
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install

# Cleanup
RUN rm -rf awscliv2.zip aws

# Copy the script to the container
COPY sync-s3-buckets.sh /usr/local/bin/sync-s3-buckets.sh

# Make the script executable
RUN chmod +x /usr/local/bin/sync-s3-buckets.sh

# Set the entrypoint to the script
ENTRYPOINT ["/usr/local/bin/sync-s3-buckets.sh"]
```

# Shellscript
```
#!/bin/bash

# Ensure IAM Role with s3 full access is attach with the EC2 instance 

# Source and destination buckets (update these with actual bucket names)
SOURCE_BUCKET="s3://staging-etl-customers-files/" #(<Source_Bucket>)
DESTINATION_BUCKET="s3://staging-etl-customer-files-us-west-1/" #(<Destination_Bucket>)

# Sync the S3 buckets
aws s3 sync $SOURCE_BUCKET $DESTINATION_BUCKET
```

 6. once all above steps done, run below commands.
    To build Docker image. 
    ```
    docker build -t s3-sync-conatainer .
    ```
    To run container using that image. 
    ```
    docker run s3-sync-conatainer > output.log 2>&1
    ``` 


