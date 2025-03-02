# Publish Windows EC2 metrics to CloudWatch

#aws #ec2 #cloudwatch #metrics #json

-----

Ensure that [[install-cloudwatchagent|CloudWatch Agent is installed]] on the instance.

Apply the following configuration to collection disk & memory info.  The CPU metrics are provided by default and require no additional configuration.

`C:\ProgramData\Amazon\CloudWatchAgent\amazon-cloudwatch-agent.json`

```json
{
	"metrics": {
		"append_dimensions": {
			"AutoScalingGroupName": "${aws:AutoScalingGroupName}",
			"ImageId": "${aws:ImageId}",
			"InstanceId": "${aws:InstanceId}",
			"InstanceType": "${aws:InstanceType}"
		},
		"metrics_collected": {
			"LogicalDisk": {
				"measurement": [
					"% Free Space"
				],
				"metrics_collection_interval": 60,
				"resources": [
					"*"
				]
			},
			"Memory": {
				"measurement": [
					"% Committed Bytes In Use"
				],
				"metrics_collection_interval": 60
			},
			"statsd": {
				"metrics_aggregation_interval": 60,
				"metrics_collection_interval": 10,
				"service_address": ":8125"
			}
		}
	}
}
```
