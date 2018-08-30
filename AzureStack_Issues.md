## Azure Stack Issues

### Issue #1
Error: unable to get local issuer certificate.

#### Solution
To ignore SSL error set a Variable of name VSTS_ARM_REST_IGNORE_SSL_ERRORS with value: true in the release definition

Reference - [VSTS task failed with certificate issue](https://stackoverflow.com/questions/50981525/vsts-tasks-failed-with-unable-to-get-local-issuer-certificate) 
