# Collect Windows Security event logs with CloudWatch Agent

#aws #cloudwatch #security #windows 

-----

Include the following JSON in the CloudWatchAgent configuraton file:

```json
"logs" : {
		"logs_collected": {
			"windows_events": {
				"collect_list": [
					{
						"event_name": "Security",
						"event_levels": [
								"VERBOSE",
								"INFORMATION",
								"WARNING",                
								"ERROR",
								"CRITICAL"
						],
						"log_group_name": "tns/security-event-logs/domain-controllers",
						"log_stream_name": "{instance_id} {local_hostname}"
					}
				]
			}
		}
 	}
```
