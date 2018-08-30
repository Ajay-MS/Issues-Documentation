## SQL Issues

### Issue #1
System.Management.Automation.ParentContainsErrorRecordException: *** Deployment cannot continueAn error occurred during deployment plan generation. 

#### Cause
Mismatch in target planform of target server and target platform being specified in dacpac file. 

#### Solution
Following are the possible solutions
* Change target platform of dacpac file as same as platform of target server. 
* Pass additional argument in task `/p:AllowIncompatiblePlatform=true`


### Issue #2
System.Management.Automation.ParentContainsErrorRecordException: *** Could not deploy package.

