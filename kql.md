# KQL Cheat Sheet

## Querying Azure Sentinel Logs Using Kusto Query Language

This page is all about KQL functions to speed up analysis in Azure Sentinel. From simple every-day queries to more advanced alert correlations.


### Basic queries
```
| where TimeGenerated >= ago(90d)
| where TimeGenerated between(datetime("2020-12-22 00:00:00") .. datetime("2020-12-22 08:00:00"))

| sort by TimeGenerated asc

let timeframe = 90d;
OfficeActivity
| where TimeGenerated >= ago(timeframe)

| summarize count() by AlertName
| where count_ > 4
| sort by count_ desc 

| project-rename new_column_name = column_name
| where User_Account_ !contains "Johny.Bravo" and isnotempty(User_Account_)

| distinct
```

### Listing number of security alerts per entity
Previously done through JSON parsing but now we can simply summarize by CompromisedEntity.

```
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Count=count() by CompromisedEntity
| sort by Count desc 
```
### JSON extend with concat
Concat function to marge User Account with UPN Suffix and "@" in between.

```
SecurityAlert
// | where SystemAlertId contains "xxxxxx-xxx-xxx-xxx-xxxxxxx"
| extend UPNSuffix_ = tostring(parse_json(Entities)[0].UPNSuffix)
| extend Name_ = tostring(parse_json(Entities)[0].Name)
| extend User_Account_ = strcat(Name_, "@" ,UPNSuffix_)
```

### Attempts to list users or groups using Net commands
Hunting like query to see who is using Net commands. Please make sure that you have enabled MDATP connecter and onboarded all needed machines. Otherwise you will not find much.

```
DeviceProcessEvents 
| where TimeGenerated > ago(14d) 
| where FileName == 'net.exe' and AccountName != "" and ProcessCommandLine !contains '\\' and ProcessCommandLine !contains '/add' 
| where (ProcessCommandLine contains ' user ' or ProcessCommandLine contains ' group ') and (ProcessCommandLine contains ' /do' or ProcessCommandLine contains ' /domain') 
| extend Target = extract("(?i)[user|group] (\"*[a-zA-Z0-9-_ ]+\"*)", 1, ProcessCommandLine) | filter Target != '' 
| project AccountName, Target, ProcessCommandLine, DeviceName, TimeGenerated 
| sort by AccountName, Target
```

### Parsing external data sets
If you need to process through Azure Sentinel external data set (for example txt file with IP addresses).

#### Search for sign-in activity from external IP address list.

```
let timeRange = 100d;
let covidIPs = externaldata (IPAddress: string) [@"http://hackme.rocks/downloads/ipaddresses.txt"] with (ignoreFirstRecord=false);
SigninLogs
| where TimeGenerated >= ago(timeRange)
| where IPAddress in~ (covidIPs)
| project TimeGenerated , UserPrincipalName , IPAddress , ResultDescription , AppDisplayName , Location
| extend AccountCustomEntity = UserPrincipalName
// | extend IPCustomEntity = IPAddress
```

#### Search for network activity in common security log from external IP address list.

```
let timeRange = 100d;
let covidIPs = externaldata (IPAddress: string) [@"http://hackme.rocks/downloads/ipaddresses.txt"] with (ignoreFirstRecord=true);
CommonSecurityLog
| where TimeGenerated >= ago(timeRange)
| where DestinationIP in~ (covidIPs)
| extend Device = iff(DeviceName <> DeviceName, DeviceName, DeviceAddress)
| project TimeGenerated , Device, SourceIP, DestinationIP, Protocol, DestinationPort, ReceivedBytes , SentBytes , DeviceAction
// | extend IPCustomEntity = SourceIP
```





