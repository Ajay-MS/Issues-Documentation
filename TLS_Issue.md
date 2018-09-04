## TLS Issues

#### Issue : HTTP, FTP clients gettign failed while making secure connection 

###### Errors
* The remote server returned an error: 234 AUTH command ok. Expecting TLS  Negotiation.
* System.Net.WebException: The remote server returned an error: 234 AUTH command ok. Expecting TLS Negotiation.
* System.IO.IOException: Authentication failed because the remote party has closed the transport stream

###### Cause 
To make secure connection server is expecting TLS 1.2 for communication while client has TLS version less than 1.2

###### Solution

Add TLS 1.2 support to Ftp/Http Client

In PowerShell TLS 1.2 can be added to client as follow
```
 $securityProtocol=@()
 $securityProtocol+=[Net.ServicePointManager]::SecurityProtocol
 $securityProtocol+=[Net.SecurityProtocolType]3072
 [Net.ServicePointManager]::SecurityProtocol=$securityProtocol
```

###### Note
If you are on Windows 2012 R2 or Windows 8 then install one of following KB to support TLS 1.2
* Windows 2012 R2, Windows 8.1: https://support.microsoft.com/en-us/kb/2898850 
* Windows 2012, Windows 8: https://support.microsoft.com/en-us/kb/2898849

After installing above mentioned KB, you need to restart your system.
