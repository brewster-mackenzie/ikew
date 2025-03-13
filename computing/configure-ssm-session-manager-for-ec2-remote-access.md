# Configure SSM Session Manager for EC2 remote access

#aws #ssm #ec2 #security

-----

## Documentation

The standard configuration for SSM Session Manager is well documented 
in the [AWS documentation](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html)

## Summary

1. create VPC endpoints
    - ec2messages.region.amazonaws.com
    - ssm.region.amazonaws.com
    - ssmmessages.region.amazonaws.com
1. install SSM Agent on all EC2 instances
1. install AWSCLIV2 in all EC2 instances
1. add the AmazonSSMManagedInstanceCore policy the role assigned to each EC2 instance

## Usage

Start an SSM session with the following command:

```bash
aws ssm start-session --target instance-id
```

For RDP it is necessary to forward remote port 3389 to a local port of your choice, then connect to local port using Microsoft Remote Desktop Connection (mstsc.exe) or a similar tool.

```bash
aws ssm start-session --target instance-id --document-name AWS-StartPortForwardingSession --parameters portNumber="3389",localPortNumber="56789"
```
