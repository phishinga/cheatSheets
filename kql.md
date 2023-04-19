# KQL Cheat Sheet

## Querying Azure Sentinel Logs Using Kusto Query Language

This page is all about KQL functions to speed up analysis in Azure Sentinel. From simple every-day queries to more advanced alert correlations.

'''
MOST USED QUERIES
=========================================
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
'''
