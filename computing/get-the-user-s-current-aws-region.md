# Get the user's current AWS region

#aws #bash 

-----

A useful command to get the current executing region if you're not sure,
for example if `AWS_REGION` and `AWS_DEFAULT_REGION` are both set.

```bash
aws ec2 describe-availability-zones --output text --query 'AvailabilityZones[0].[RegionName]'
```
