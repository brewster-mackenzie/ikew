# Report on CloudWatch alarms in Alarm status

Use PowerShell to generate a CSV report of all CloudWatch alarms currently in Alarm status.  Modify the `$regions` variable to select which regions to report on.

```powershell
$regions = 'eu-west-1', 'us-west-2', 'ap-southeast-2'

foreach($region in $regions) {
    Invoke-Command {
        Write-Host "Gathering data for region $region..."
        $alarms = aws cloudwatch describe-alarms --state-value ALARM --region $region --output json --no-paginate | ConvertFrom-Json | Select-Object -ExpandProperty MetricAlarms
        $alarms | Select-Object AlarmName, AlarmARN, MetricName, StateUpdatedTimestamp, StateValue, StateReason
    } | ConvertTo-CSV -NoTypeInformation | Set-Content "CWAlarmsInAlarmStatus_$region.csv"  
}
```
