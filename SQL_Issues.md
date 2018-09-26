## SQL Info and Issues

## SQL Documentations
* [SQLPackage - Actions and arguments](https://docs.microsoft.com/en-us/sql/tools/sqlpackage?view=sql-server-2017)
* [Azure SQL database deployment - Supported Actions](https://docs.microsoft.com/en-us/vsts/pipelines/targets/azure-sqldb?branch=master&view=vsts&tabs=yaml)
* [SQLCMD arguments](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-2017)
* [Invoke-SQLCMD arguments](https://docs.microsoft.com/en-us/powershell/module/sqlps/invoke-sqlcmd?view=sqlserver-ps)
* [Compare Invoke-SQLCMD and SQLCMD arguments](https://docs.microsoft.com/en-us/sql/database-engine/invoke-sqlcmd-cmdlet?view=sql-server-2014#comparing-invoke-sqlcmd-and-the-sqlcmd-utility)

#### Powershell Script to connnect to Azure SQL with SPN
```
$TenantId = "<Tenant Id>"
$SPNName = "<Enter application display name>"
$SPNPassword = "<Enter SPN Key>"
$SQLServerName = "<SQL Server name>"

$ServicePrincipalId = $(Get-AzureRmADServicePrincipal -DisplayName "$SPNName").ApplicationId
$SecureStringPassword = ConvertTo-SecureString -String $SPNKey -AsPlainText -Force

Get-AADToken -TenantID $TenantId -ServicePrincipalId $ServicePrincipalId -ServicePrincipalPwd $SecureStringPassword -OutVariable SPNToken

Write-Verbose "Create SQL connectionstring"
$conn = New-Object System.Data.SqlClient.SQLConnection 
$DatabaseName = 'Master'
$conn.ConnectionString = "Data Source=$SQLServerName.database.windows.net;Initial Catalog=$DatabaseName;Connect Timeout=30"
$conn.AccessToken = $($SPNToken)
$conn

Write-Verbose "Connect to database and execute SQL script"
$conn.Open() 
$query = 'select @@version'
$command = New-Object -TypeName System.Data.SqlClient.SqlCommand($query, $conn) 	
$Result = $command.ExecuteScalar()
$Result
$conn.Close() 
```

## Issues

### Issue - Deployment cannot continueAn error occurred during deployment plan generation. 

#### Cause
Mismatch in target planform of target server and target platform being specified in dacpac file. 

#### Solution
Following are the possible solutions
* Change target platform of dacpac file as same as platform of target server. 
* Pass additional argument in task `/p:AllowIncompatiblePlatform=true`

### Issue #2 - Unable to connect to the SQL server
* System.Management.Automation.ParentContainsErrorRecordException: *** Could not deploy package.
* Unable to connect to the SQL server. 

#### Suggestion
Please check below questions
* Is SQL server name provided as FQDN?
* Is firewall port opened in your SQL server?
* Is username and password provided correctly?
* Is there any proxy server in your network?
* Are you able to connect to your SQL server using SSMS?

### Issue #3 - Azure SQL DACPAC timeout

Try below mentioned ways of resolving this issue
##### Increase timeout
* For SQLPackage.exe, Command and Target timeout can be increased by passing additional arguments as
`/p:CommandTimeout=1800 /TargetTimeout: 1800`

* For Invoke-SqlCmd, Connection and Query timeout can be increase by passing additional arguments as 
`-ConnectionTimeout=1200 -QueryTimeout=1200`

##### Set LongRunningQueryTimeoutSeconds in registry
* Run PowerShell with administrator permission and execute below mentioned command
`C:\Windows\System32\reg.exe add HKCU\Software\Microsoft\VisualStudio\10.0\SQLDB\Database /v LongRunningQueryTimeoutSeconds /t REG_DWORD /d 0 /f`

##### Collect diagnostic logs 
If issue didn't get solved with steps mentioned above, collect diagnostics logs and share with the team. 

To colelct diagnostics logs provide below argument in additional arguments of task
`/df :<logfilepath>`

###### How to get diagnostic logs on hosted agent

Add argument for generating diagnostic logs in additional argument input of Azure SQL dacpac task
`/df:'D:\sqlpackage.log'`

Add PowerShell task next Azure SQL Dacpac task, select inline script as the option and copy below script
`Get-Content 'D:\sqlpackage.log'`
