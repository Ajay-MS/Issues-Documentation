## Powershell Issue 

#### Issue -  Inline script in Azure Powershell Task doesn't accept additional arguments 
You can access your release variable directly in your script by specifying in the following format.
`$(<VARIABLE_NAME>)`

e.g. if you have defined variable `username` in the release. In the inline script, it could be accessed as `$(username)`
