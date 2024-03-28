**Use the keys in uploader-service.js and AWS.config.update**
```python
cat /home/sec588/Coursefiles/workdir/uploader-service.js | grep AWS.config.update | head -1
```

**Use the aws configure option to configure the profile to use for this exercise. When you are asked for a Default output format, just hit ENTER**
```python
aws configure --profile lab25X
```

**Next we need to figure out the resource or user attached to the key. This command does not leave a log trail so that we can run it safely**
```python
aws sts get-caller-identity --profile lab25X
```

You should see a UserID with the key value starting with AIDA (User Id Resource). The ARN shows us the value of a user resource.

**Determine if the user can list out all the S3 buckets**
```python
aws s3 ls --profile lab25X
```

The error messages tell us the user cannot list all buckets because they lack the API permission for s3:ListBuckets.
```python
sec588@slingshot:~/Coursefiles/workdir$ aws s3 ls --profile lab25X

An error occurred (AccessDenied) when calling the ListBuckets operation: Access Denied
```

**Try one of the buckets in uploader-service.js and see if we can list buckets in that bucket directly**
```python
aws s3 ls s3://pictures.awesome-sparrow.sec588.cloud/ --profile lab25X
```

**Copy the bucket contents to try and get contents out of the bucket**
```python
aws s3 cp --profile lab25X s3://pictures.awesome-sparrow.sec588.cloud/raven.png /home/sec588/Coursefiles/workdir/raven.png
```
The below output means the user can list the bucket's contents but cannot download the bucket's content:
```python
sec588@slingshot:~/Coursefiles/workdir$ aws s3 cp --profile lab25X s3://pictures.awesome-sparrow.sec588.cloud/raven.png /home/sec588/Coursefiles/workdir/raven.png
fatal error: An error occurred (403) when calling the HeadObject operation: Forbidden
```

The a



