# Report on AWS Security Groups and Rules

#aws #vpc #security #powershell

Use PowerShell to generate a CSV report of all AWS security groups and rules in the given regions.

Replace `$regions` in the script to select which regions should be included.

The output is non-standard CSV nested tables, e.g.

```
GroupId, GroupName, Description 
my-group-id, my-group-name, my-group-description
RuleId, IpProtocol, CidrIpv4, FromPort, ToPort, Description
my-rule-id-1, my-rule-proto-1, my-rule-cidr-1, my-rule-from-1, my-rule-to-1, my-desc-1
my-rule-id-2, my-rule-proto-2, my-rule-cidr-2, my-rule-from-2, my-rule-to-2, my-desc-2
my-rule-id-3, my-rule-proto-3, my-rule-cidr-3, my-rule-from-3, my-rule-to-3, my-desc-3
```

And is saved to the file `SecurityGroup_$region.csv`

```powershell
$regions = 'eu-west-1', 'us-west-1'

foreach($region in $regions) {
  Invoke-Command {
    $groups = aws ec2 describe-security-groups --region $region --output json --no-paginate | ConvertFrom-Json | Select-Object -ExpandProperty SecurityGroups
    $ruleGroups = aws ec2 describe-security-group-rules --region $region --output json --no-paginate | ConvertFrom-Json | Select-Object -ExpandProperty SecurityGroupRules | Group-Object GroupId
    
    foreach($ruleGroup in $ruleGroups) {
      $group = $groups | Where-Object GroupId -EQ $ruleGroup.Name
      if ($group) {
        $group | Select-Object GroupId, GroupName, Description | ConvertTo-Csv -NoTypeInformation        
        $ruleGroup.Group | Select-Object SecurityGroupRuleId, IpProtocol, CidrIpv4, FromPort, ToPort, Description | ConvertTo-Csv -NoTypeInformation        
      }

      "" # separate groups with blank line
    }
  } | Set-Content "SecurityGroup_$region.csv"  
}
```
